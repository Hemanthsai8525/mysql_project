# mysql_project
mysql_project

SELECT * FROM instagram_clone_db.users;

-- 1. find top 5 users who have join early.
select * from users order by created_at asc limit 5;
-- 2. find the days users created there accounts.
select dayname(created_at) as day_of_the_week, count(*) as total_account from users group by day_of_the_week order by total_account desc;
-- 3. find the user who have never posted a photo.
select users.id,users.username,users.created_at as users_joining_date
from users 
left join photos on users.id=photos.user_id
where photos.user_id is null;

-- 4. find the most likes for a single photo.
select users.id,users.username ,photos.id,photos.image_url,count(*) as total_likes from photos
join likes on photos.id=likes.photo_id
join users on photos.user_id=users.id
group by photos.id order by total_likes desc limit 1;

-- 5. find no of users who likes every single photo on the site.
select users.id as id,users.username as username, count(*) as total_likes from users join likes on users.id=likes.user_id
group by users.id having total_likes = (select count(*) from photos);

-- 6. what are the top 5 most commonly used hastags.
select tags.id,tags.tag_name, count(*) as total_tags from tags
join photo_tags on tags.id=photo_tags.tag_id group by tag_id order by total_tags desc limit 5;
