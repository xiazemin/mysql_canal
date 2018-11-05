canal的原理是模拟自己为mysql slave，所以这里一定需要做为mysql slave的相关权限.

CREATE USER canal IDENTIFIED BY 'canal';  

GRANT SELECT, REPLICATION SLAVE, REPLICATION CLIENT ON \*.\* TO 'canal'@'%';

-- GRANT ALL PRIVILEGES ON \*.\* TO 'canal'@'%' ;

FLUSH PRIVILEGES;

