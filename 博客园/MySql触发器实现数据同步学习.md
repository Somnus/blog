<p>触发器实现：（增、删、改操作事件触发数据单向同步）<br />数据库触发器教程：https://www.cnblogs.com/phpper/p/7587031.html<br />同步代码：</p>
<div class="cnblogs_code">
<pre><span style="color: #000000;">DELIMITER $
DROP TRIGGER IF EXISTS insertdata;
CREATE TRIGGER insertdata AFTER INSERT ON repair.user FOR EACH ROW
BEGIN
DECLARE s1 INT(</span><span style="color: #800080;">11</span><span style="color: #000000;">);
DECLARE s2 VARCHAR(</span><span style="color: #800080;">30</span><span style="color: #000000;">);
SET s1</span>=<span style="color: #000000;">NEW.id;
SET s2</span>=<span style="color: #000000;">NEW.name;
INSERT INTO sale.user(id,name) VALUES(s1,s2);
END $

DELIMITER $
DROP TRIGGER IF EXISTS updatedata;
CREATE TRIGGER updatedata AFTER UPDATE ON repair.user FOR EACH ROW
BEGIN
DECLARE s1 INT(</span><span style="color: #800080;">11</span><span style="color: #000000;">);
DECLARE s2 VARCHAR(</span><span style="color: #800080;">30</span><span style="color: #000000;">);
SET s1</span>=<span style="color: #000000;">OLD.id;
SET s2</span>=<span style="color: #000000;">NEW.name;
UPDATE sale.user a SET a.name</span>=s2 WHERE a.id=<span style="color: #000000;">s1;
END $

DELIMITER $
DROP TRIGGER IF EXISTS deletedata;
CREATE TRIGGER deletedata AFTER DELETE ON repair.user FOR EACH ROW
BEGIN
DECLARE s1 INT(</span><span style="color: #800080;">11</span><span style="color: #000000;">);
DECLARE s2 VARCHAR(</span><span style="color: #800080;">30</span><span style="color: #000000;">);
SET s1</span>=<span style="color: #000000;">OLD.id;
SET s2</span>=<span style="color: #000000;">OLD.name;
DELETE FROM sale.user WHERE id</span>=<span style="color: #000000;">s1;
END $</span></pre>
</div>
<p>&nbsp;</p>