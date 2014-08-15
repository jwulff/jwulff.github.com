---
layout: post
title: PostgreSQL List Key References
---

Today I was trying to merge a bunch of user records in our application. A lot 
of tables in our database reference user records (`belongs_to :user`). To 
merge users 2 and 3 into user 1 all references in the database to users 2 and 
3 must be updated to user 1 and then users 2 and 3 need to be deleted.
 
Because we make diligent use of PostgreSQL's wonderful referential integrity 
constraints I was able to inspect the `information_schema` to find all tables 
that reference the `users.id` column.

    SELECT
      tc.table_name, kcu.column_name
    FROM
     information_schema.constraint_column_usage as ccu
     JOIN information_schema.table_constraints AS tc ON (ccu.constraint_name = tc.constraint_name)
     JOIN information_schema.key_column_usage AS kcu on (ccu.constraint_name = kcu.constraint_name)
    WHERE
      ccu.table_name = 'users'
      AND tc.constraint_type = 'FOREIGN KEY';

Result:

             table_name         |  column_name  
    ----------------------------+---------------
     message_recipients         | recipient_id
     messages                   | sender_id
     user_roles                 | user_id
     user_sessions              | user_id
     user_sessions              | super_user_id
     user_settings              | user_id

Then it was simply a matter of doing a find and replace on those columns:

    UPDATE message_recipients SET recipient_id  = 1 WHERE recipient_id  IN (2, 3)
    UPDATE messages           SET sender_id     = 1 WHERE sender_id     IN (2, 3)
    UPDATE user_roles         SET user_id       = 1 WHERE user_id       IN (2, 3)
    UPDATE user_sessions      SET user_id       = 1 WHERE user_id       IN (2, 3)
    UPDATE user_sessions      SET super_user_id = 1 WHERE super_user_id IN (2, 3)
    UPDATE user_settings      SET user_id       = 1 WHERE user_id       IN (2, 3)

Probably should write a method to do this. Here it is:

    class User < ActiveRecord::Base
      
      ...
      
      # Changes all refereces to `other_user` to self and deletes `other_user`
      def merge!(other_user)
        sql = "SELECT
          tc.table_name, kcu.column_name
        FROM
         information_schema.constraint_column_usage as ccu
         JOIN information_schema.table_constraints AS tc ON (ccu.constraint_name = tc.constraint_name)
         JOIN information_schema.key_column_usage AS kcu on (ccu.constraint_name = kcu.constraint_name)
        WHERE
          ccu.table_name = 'users'
          AND tc.constraint_type = 'FOREIGN KEY';"
        ActiveRecord::Base.transaction do
          ActiveRecord::Base.connection.execute(sql).each do |row|
            update_sql = "UPDATE #{row['table_name']} SET #{row['column_name']} = #{id} WHERE #{row['column_name']} = #{other_user.id}"
            ActiveRecord::Base.connection.execute update_sql
          end
          ActiveRecord::Base.connection.execute "DELETE FROM users WHERE id = #{other_user.id}"
        end
      end
    end