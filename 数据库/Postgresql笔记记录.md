



# **Postgresql相关笔记

------



## 一、PostGIS相关笔记

**ST_AsText**

图形转Text ，图形WKTST_AsEwkt，点XY获取函数ST_X

```sql
select t.gid,ST_AsText(t.geom),ST_AsEwkt(t.geom),ST_X(t.geom),ST_Y(t.geom) from point t
```



**ST_Centroid**

--面质心获取函数

```sql
select t.gid,ST_AsText(t.geom),ST_AsEwkt(t.geom),ST_AsText(ST_Centroid(t.geom)) from polygon t
```



**ST_Distance_Sphere**

--图形球面距离函数

```sql
SELECT p1.gid,p2.gid,ST_Distance_Sphere(p1.geom,p2.geom) FROM point AS p1, point AS p2 WHERE p1.gid > p2.gid;
```



**GeometryType**

--获取图形类型函数

```sql
select t.gid,ST_AsText(t.geom),ST_AsEwkt(t.geom),ST_X(t.geom),ST_Y(t.geom),GeometryType(t.geom) from point t
```



**ST_GeomFromText**

--文本转图形函数，无SRID

```sql
select ST_AsText(ST_GeomFromText('POINT(62.2099990844727 34.4599990844727)')),t.* from point t
```

--文本转图形函数，有SRID

```sql
select ST_GeomFromText('SRID=4326;POINT(62.2099990844727 34.4599990844727)'),t.* from point t
```

**ST_Distance**

--几何距离笛卡儿距离

```sql
select t.gid,ST_Distance(ST_GeomFromText('SRID=4326;POINT(62.2099990844727 34.4599990844727)'),t.geom) from point t
```

**ST_Distance_Sphere**

--球面距离

```sql
select t.gid,ST_Distance_Sphere(ST_GeomFromText('SRID=4326;POINT(62.2099990844727 34.4599990844727)'),t.geom) from point t
```

 

**ST_DWithin(geometry, geometry, float)** 

--如果一个几何对象(geometry)在另一个几何对象描述的距离(float)内，返回TRUE。如果有索引，会用到索引。 

```sql
select ST_DWithin(t.geom,ST_GeomFromText('SRID=4326;POINT(62.2099990844727 34.4599990844727)'),5) from poin
```

 

**ST_Equals(geometry, geometry)**

--如果两个空间对象相等，则返回TRUE。用这个函数比用“=”更好，例如：

--equals('LINESTRING(0 0, 10 10)','LINESTRING(0 0, 5 5, 10 10)') 返回 TRUE。

```sql
select t.gid,ST_Equals(ST_GeomFromText('SRID=4326;POINT(62.2099990844727 34.4599990844727)'),ST_GeomFromText('SRID=4326;POINT(62.2099990844727 34.4599990844727)')) from point t
```

```sql
SELECT p1.gid,p2.gid,ST_Equals(p1.geom,p2.geom) FROM point AS p1, point AS p2 WHERE p1.gid = p2.gid;
```

 

**ST_Touches(geometry, geometry)**

--如果两个几何空间对象存在接触，则返回TRUE。不要使用GeometryCollection作为参数。

--a.Touches(b) -> (I(a) intersection I(b) = {empty set} ) and (a intersection b) not empty

--不使用索引可以用_ST_Touches.

```sql
SELECT p1.gid,p2.gid,ST_Touches(p1.geom,p2.geom) FROM polygon AS p1, polygon AS p2 WHERE p1.gid=5 and p2.gid=2
```

 

**ST_Disjoint(geometry, geometry)**

--如果两个对象不相连，则返回TRUE。不要使用GeometryCollection作为参数。

```sql
SELECT p1.gid,p2.gid,ST_Disjoint(p1.geom,p2.geom) FROM polygon AS p1, polygon AS p2 WHERE p1.gid > p2.gid;
```

 

**ST_Intersects(geometry, geometry)**

--判断两个几何空间数据是否相交,如果相交返回true,不要使用GeometryCollection作为参数。

--Intersects(g1, g2 ) --> Not (Disjoint(g1, g2 ))

--不使用索引可以用_ST_Intersects.

```sql
select t.gid,ST_Intersects(ST_GeomFromText('SRID=4326;POINT(62.2099990844727 34.4599990844727)'),t.geom) from polygon t
```

 

**ST_Touches(geometry, geometry)**

--如果两个几何空间对象存在接触，则返回TRUE。不要使用GeometryCollection作为参数。

--a.Touches(b) -> (I(a) intersection I(b) = {empty set} ) and (a intersection b) not empty

--不使用索引可以用_ST_Touches.

```sql
SELECT p1.gid,p2.gid,ST_Touches(p1.geom,p2.geom) FROM polygon AS p1, polygon AS p2 WHERE p1.gid > p2.gid;
```

 

**ST_Crosses(geometry, geometry)**

--如果两个几何空间对象存在交叉，则返回TRUE。不要使用GeometryCollection作为参数。

--不使用索引可以用_ST_Crosses.

```sql
SELECT p1.gid,p2.gid,ST_Crosses(p1.geom,p2.geom) FROM line AS p1, line AS p2 WHERE p1.gid > p2.gid;
```

 

**ST_Within(geometry A, geometry B)**

如果几何空间对象A存在空间对象B中,则返回TRUE,不要使用GeometryCollection作为参数。

不使用索引可以用_ST_Within

 

**ST_Overlaps(geometry, geometry)**

如果两个几何空间数据存在交迭,则返回 TRUE,不要使用GeometryCollection作为参数。

不使用索引可以用_ST_Overlaps.

 

**ST_Contains(geometry A, geometry B)**

如果几何空间对象A包含空间对象B,则返回 TRUE,不要使用GeometryCollection作为参数。

这个函数类似于ST_Within(geometry B, geometry A)

不使用索引可以用_ST_Contains.

 

**ST_Covers(geometry A, geometry B)**

如果几何空间对象B中的所有点都在空间对象A中,则返回 TRUE。

不要使用GeometryCollection作为参数。

不使用索引可以用_ST_Covers.



**ST_CoveredBy(geometry A, geometry B)**

如果几何空间对象A中的所有点都在空间对象B中,则返回 TRUE。*/

**ST_Centroid(geometry)**

返回质心点,就是根据几何空间数据,活动该几何空间数据的中心点,返回一个空间点数据.

**ST_Area(geometry)**

如果几何空间数据为多边形,或者多多边形,则返回空间数据的外围(返回类型double precision) ;

**ST_Length(geometry)**

这个曲线在其相关的空间参考长度(返回类型double precision) ;

**ST_PointOnSurface(geometry)**

一定在几何空间线数据上的点，返回一个数据点*/

```sql
select t.gid,ST_AsText(ST_Centroid(t.geom)),ST_Area(t.geom),ST_Length(t.geom),ST_AsText(ST_PointOnSurface(t.geom)) from polygon t
```

 **ST_Buffer(geometry, double, [integer])**

--buffer操作一个很有用函数，

--这个函数的第一个参数是要操作的空间几何数据，第二个参数长度（距离），第三个参数为一个整型，

--这个函数返回一个空间数据类型，以当前第一个参数空间几何数据为参考点，返回小于等于距离的空间

**示例SQL：**

```sql
select t.gid,ST_AsText(ST_Buffer(t.geom,2)) from point t
select ST_AsGeoJSON(ST_GeomFromText('POLYGON ((92.475390624974239 40.1150177340468, 100.5613281249706 33.618244328348453, 95.644302013600225 29.451621692949303, 87.5583645136038 36.273124909000586, 92.475390624974239 40.1150177340468))'))
```

 新增空间字段

```sql
SELECT AddGeometryColumn('public', 'point', 'the_geom', 4326, 'POINT', 2);
CREATE INDEX "sidx_app_item_info_geom" ON "public"."app_item_info" USING gist (
 "geom" "public"."gist_geometry_ops_2d"
);
```

 空间索引：

```sql
CREATE INDEX "sidx_dms_geo_ctrl_airspace_geom" ON "public"."Untitled" USING gist (
 "geom" "public"."gist_geometry_ops_2d"
);
```

----GeoJson 插入图形 shape导入面

```sql
INSERT INTO tbmap_geofence (geom, id )
VALUES (ST_SetSRID(st_force2d(ST_GeomFromGeoJSON('{"type":"MultiPolygon","coordinates":[[[[113.303385707385,23.3962760086502],[113.305723979849,23.3957375758269],[113.305517692524,23.3947220504831],[113.303090286523,23.3951830060744],[113.303385707385,23.3962760086502]]]]}')), 4326), 7);
```

--圆

```sql
INSERT INTO tbmap_geofence (geom, id )

VALUES (ST_SetSRID(st_force2d(ST_GeomFromGeoJSON('{"type":"MultiPolygon","coordinates":[[[[113.32256425148469,23.385729789733887],...,[113.32256425148469,23.385729789733887]]]]}')), 4326), 9);
```

--多边形模板

```sql
INSERT INTO tbmap_geofence (geom, id ) VALUES (ST_SetSRID(st_force2d(ST_GeomFromGeoJSON('{"type":"MultiPolygon","coordinates":["+--坐标串-+"]}')), 4326), 8);
INSERT INTO tbmap_geofence (geom, id )

VALUES (ST_SetSRID(st_force2d(ST_GeomFromGeoJSON('{"type":"MultiPolygon","coordinates":[[[[113.29585094553487,23.37813377380371],…],[113.29584962899072,23.378049966894025],[113.29585094553487,23.37813377380371]]]]}')), 4326), 10);
select t.gid,ST_AsGeoJSON(t.geom) from tbmap_geofence t
select * from tbmap_geofence where id=1
select t.gid,ST_AsEwkt(t.geom) from tbmap_geofence t
select t.gid,ST_AsText(t.geom) from tbmap_geofence t
select count(*) from tbmap_geofence
INSERT INTO tbmap_geofence (geom, id )
VALUES (ST_SetSRID(st_force2d(ST_GeomFromText('{"type":"MultiPolygon","coordinates":[[[[113.29585094553487,23.37813377380371],..,[113.29585094553487,23.37813377380371]]]]}')), 4326), 10);
```

//更新

```sql
UPDATE tbmap_geofence SET geom=ST_GeomFromText('SRID=4326;MULTIPOLYGON(((113.303745958764 23.3871859534098,113.303982189006 23.3871366260626,113.3027236068 23.3821573252375,113.302080751185 23.3822608200131,113.303292917796 23.3872893329739,113.303745958764 23.3871859534098)))') WHERE id=1
```

//更新

```plsql
UPDATE "TBMap_GeoFence" SET geom=ST_GeomFromEWKT('SRID=4326;MULTIPOLYGON (((113.30680847167967 23.384592533111572, 113.30597162246704 23.381996154785156, 113.30751657485962 23.381288051605225, 113.30886840820312 23.382253646850582, 113.30946922302246 23.384613990783691, 113.30949068069458 23.3858585357666, 113.30811738967896 23.385772705078125, 113.30680847167967 23.384592533111572)))'),"UserID"='f7426233-92e8-46a2-9680-04dbc188c4ad' WHERE "ID"=5;
```

```sql
SELECT ST_AsEwkt(ST_SetSRID(ST_GeomFromGeoJSON('{"type":"MultiPolygon","coordinates":[[[113.31629746228292,23.39148044586182],..,[113.31629746228292,23.39148044586182]]]}'), 4326)) As wkt;
```

//创建空间表 点

```sql
CREATE TABLE "public"."Fixes" (
"gid" serial8 NOT NULL,
"Name" varchar(40),
"Latitude" float4,
"Longitude" float4,
"geom" "public"."geometry"(POINT,4326),
PRIMARY KEY ("gid")
)
WITH (OIDS=FALSE)
;
ALTER TABLE "public"."Fixes" OWNER TO "postgres";
--创建空间索引
CREATE INDEX "Fixes_geom_idx" ON "public"."Fixes" USING gist ("geom" "public"."gist_geometry_ops_2d");
INSERT INTO "Fixes" (geom)
VALUES (ST_SetSRID(ST_GeomFromGeoJSON('{"type":"Point","coordinates":[-48.23456,20.12345]}'),4326));
```

**Postgres tabel转GeoJSON**

 

```sql
SELECT
 row_to_json ( fc ) AS geojson 
FROM
 (
 SELECT
 'FeatureCollection' AS TYPE,
 array_to_json ( ARRAY_AGG ( f ) ) AS features
 FROM
 (
 SELECT
  'Feature' AS TYPE,
  ST_AsGeoJSON ( ( lg.geom ), 15, 0 ) :: json AS geometry,
  row_to_json ( ( "ID", "Name","Type") ) AS properties 
 FROM
  "TBAApp_BaseGeoEntrance" AS lg 
 ) AS f 
 ) AS fc;
```

**PostGIS 距离计算规范 - 投影 与 球 坐标系, geometry 与 geography 类型**

1、使用球坐标，通过st_distancespheroid计算两个geometry类型的球面距离。

对于geometry类型，可以使用st_distancespheroid直接用球坐标计算，在计算时会自动设置这个椭球特性（SPHEROID["Krasovsky_1940",6378245.000000,298.299997264589] ）。

```sql
postgres=# SELECT st_distancespheroid(ST_GeomFromText('POINT(120.08 30.96)', 4326),ST_GeomFromText('POINT(120.08 30.92)', 4326), 'SPHEROID["WGS84",6378137,298.257223563]');  
 st_distancespheroid   
 ---------------------  
   4434.73734584354  
 (1 row)  
```

2、使用平面坐标，通过st_distance计算两个geometry类型的平面投影后的距离。

采用精准的投影坐标（小面积投影坐标系）（但是必须要覆盖到要计算的两个点）

```sql
ST_Transform(ST_GeomFromText('POINT(120.08 30.96)', 4326), 2163 )  
```

如果geometry值的SRID不是（高精度）目标坐标系，可以使用ST_Transform函数进行转换，转换为目标投影坐标系，再计算距离。

```sql
postgres=# SELECT st_distance(ST_Transform(ST_GeomFromText('POINT(120.08 30.96)', 4326), 2390 ), ST_Transform(ST_GeomFromText('POINT(120.08 30.92)', 4326), 2390 ));  
  st_distance   
 ------------------ 
 4547.55477647394 
 (1 row) 
```

3、使用球坐标，通过ST_Distance计算两个geography类型的球面距离。

float ST_Distance(geography gg1, geography gg2, boolean use_spheroid);  -- 适用椭球体（WGS84）  
 use_spheroid设置为ture表示使用:   
  -- WGS84 椭球体参数定义  
  vspheroid := 'SPHEROID["WGS84",6378137,298.257223563]' ;  

这里的XXXX就是你要选择的球坐标系SRID。在spatial_ref_sys表里可以查看各种坐标系。

```sql
postgres=# SELECT st_distance(ST_GeogFromText('SRID=xxxx;POINT(120.08 30.96)'), ST_GeogFromText('SRID=xxxx;POINT(120.08 30.92)'), true);  
  st_distance    
 \----------------  
 xxxxxxxxxxxxxxxxxxxx  
 (1 row)  
  
 --例如 
  
 postgres=# SELECT st_distance(ST_GeogFromText('SRID=4610;POINT(120.08 30.96)'), ST_GeogFromText('SRID=4610;POINT(120.08 30.92)'), true);  
  st_distance   
 \---------------- 
 4434.739418211 
 (1 row) 
```

如果允许一定的偏差，可以使用全球球坐标系4326。

```sql
postgres=# SELECT st_distance(ST_GeogFromText('SRID=4326;POINT(120.08 30.96)'), ST_GeogFromText('SRID=4326;POINT(120.08 30.92)'), true);  
  st_distance    
 ----------------  
 4434.737345844  
 (1 row)  
```

geography只支持球坐标系，使用投影坐标会报错。

```sql
postgres=# SELECT st_distance(ST_GeogFromText('SRID=2369;POINT(120.08 30.96)'), ST_GeogFromText('SRID=2369;POINT(120.08 30.92)'), true);  
 错误: 22023: Only lon/lat coordinate systems are supported in geography. 
 LOCATION: srid_is_latlong, lwgeom_transform.c:774 
```

4、指定SPHEROID内容，通过st_distancesphereoid计算geometry的球面距离。

这种方法最为精确，但是要求了解计算距离当地的地形属性（即输入参数的spheroid的内容）。

```sql
db1=# SELECT st_distancespheroid(ST_GeomFromText('POINT(120.08 30.96)', 4326),ST_GeomFromText('POINT(120.08 30.92)', 4326), 'SPHEROID["WGS84",6378137,298.257223563]');    
 st_distancesphere    
 -------------------   
   4447.803189385   
 (1 row)   
```

小结

计算距离，应该考虑到被计算的两点所在处的地球特性（spheroid）。这样计算得到的距离才是最精确的。

geometry和geography类型的选择，建议使用geometry，既能支持球坐标系，又能支持平面坐标系。主要考虑到用户是否了解位置所在处的地理特性，选择合适的坐标系。

-- 适用平面直角坐标，适用geometry类型，计算直线距离  
 float ST_Distance(geometry g1, geometry g2);       

-- 适用椭球体坐标，适用geography类型，计算球面距离 
 float ST_Distance(geography gg1, geography gg2);     

-- 适用椭球体坐标（WGS84），适用geography类型，计算球面距离  
 float ST_Distance(geography gg1, geography gg2, boolean use_spheroid);   

  use_spheroid设置为ture表示使用:   
   vspheroid := 'SPHEROID["WGS84",6378137,298.257223563]' ; -- WGS84 椭球体参数定义   

-- 适用椭球体坐标以及投影坐标，适用geometry类型，求球面距离（需要输入spheroid） 
 float ST_DistanceSpheroid(geometry geomlonlatA, geometry geomlonlatB, spheroid measurement_spheroid);  

**PostGIS矢量切片函数**

```sql
WITH mvtgeom AS
 (
  SELECT ST_AsMVTGeom(geom, ST_TileEnvelope(12,513,412)) AS geom, name, description
  FROM points_of_interest
  WHERE ST_Intersects(geom, ST_TileEnvelope(12,513,412)
 )
 SELECT ST_AsMVT(mvtgeom.*)
 FROM mvtgeom;
```

PostGIS 3857 最大纬度 85.051130064 

```sql
SELECT
	(
	SELECT
		ST_AsMVT ( tile1, 'lines', 4096, 'geom' ) 
	FROM
		(
		SELECT
			display,
			ST_AsMVTGeom ( geom, ST_MakeEnvelope ( : lonmin,: latmin,: lonmax,: latmax, 4326 ), 4096, 256, TRUE ) AS geom 
		FROM
			PUBLIC.app_geo_graticule 
		) AS tile1 
		) || (
	SELECT
		ST_AsMVT ( tile2, 'lines2', 4096, 'geom' ) 
	FROM
		(
		SELECT
			​ ADMIN,
			ST_AsMVTGeom ( geom, ST_MakeEnvelope ( : lonmin,: latmin,: lonmax,: latmax, 4326 ), 4096, 256, TRUE ) AS geom 
		FROM
			PUBLIC.app_geo_country_line 
		) AS tile2 
	) AS tile
```

 

最终：

```sql
SELECT
	(
		(
		SELECT
			ST_AsMVT ( geo_graticule, 'geo_graticule', 4096, 'geom' ) 
		FROM
			(
			SELECT
				display,
				ST_AsMVTGeom ( ST_Transform ( geom, 3857 ), ST_TileEnvelope ( 4, 14, 5 ), 4096, 256, TRUE ) AS geom 
			FROM
				PUBLIC.app_geo_graticule 
			WHERE
				ST_Transform ( st_envelope ( geom ), 3857 ) && ST_TileEnvelope ( 4, 14, 5 ) 
			) AS geo_graticule 
			) || (
		SELECT
			ST_AsMVT ( geo_country_line2, 'geo_country_line2', 4096, 'geom' ) 
		FROM
			(
			SELECT
				'admin',
				ST_AsMVTGeom ( ST_Transform ( geom, 3857 ), ST_TileEnvelope ( 4, 14, 5 ), 4096, 256, TRUE ) AS geom 
			FROM
				PUBLIC.app_geo_country_line2 
			WHERE
				ST_Transform ( st_envelope ( geom ), 3857 ) && ST_TileEnvelope ( 4, 14, 5 ) 
			) AS geo_country_line2 
		) 
	) AS tile
```

采用空间索引优化：

```sql
CREATE INDEX "sidx_dms_geo_enroute_geom" ON "public"."Untitled" USING gist ( "geom" "public"."gist_geometry_ops_2d" );
EXPLAIN ANALYZE SELECT
	routeid,
	routedisfr,
	minalt1,
	icaocode,
	ST_AsMVTGeom ( ST_Transform ( geom, 3857 ), ST_TileEnvelope ( 9, 423, 200 ), 4096, 256, TRUE ) AS geom 
FROM
	PUBLIC.dms_geo_enroute 
WHERE
	ST_Intersects ( geom, ST_Transform ( ST_TileEnvelope ( 9, 423, 200 ), 4326 ) )
```

 

 

```sql
--注意：PostGIS 3.0 可以直接使用
--PostGIS 2.0需要以下步骤：
CREATE 
	OR REPLACE FUNCTION BBox ( x INTEGER, y INTEGER, zoom INTEGER ) RETURNS geometry AS $BODY$ DECLARE
	MAX NUMERIC := 6378137 * pi( );
res NUMERIC := MAX * 2 / 2 ^ zoom;
bbox geometry;
BEGIN
		RETURN st_transform (
			ST_MakeEnvelope (
				- MAX + ( x * res ),
				  MAX - ( y * res ),
				- MAX + ( x * res ) + res,
				  MAX - ( y * res ) - res,
				  3857 
			),
			4326 
		);
END;

$BODY$ LANGUAGE plpgsql IMMUTABLE;
SELECT
	ST_AsMVT ( q, 'admin', 4096, 'geom' ) 
FROM
	(
	SELECT ID
		,
		NAME,
		admin_level,
		ST_AsMvtGeom ( geometry, BBox ( 16597, 11273, 15 ), 4096, 256, TRUE ) AS geom 
	FROM
		import.osm_admin 
	WHERE
		geometry && BBox ( 16597, 11273, 15 ) 
		AND ST_Intersects ( geometry, BBox ( 16597, 11273, 15 ) ) 
	) AS q;
--来自 <https://www.jianshu.com/p/24f7363c05b9>
SELECT
	st_xmin ( BBox ( 106790, 56773, 17 ) ) SELECT
	ST_AsMvtGeom ( geom, ST_MakeEnvelope ( Box( 106790, 56773, 17 ) ), 4096, 256, TRUE ) AS geom2 
FROM
	geo_vec_merge_surfaces 
WHERE
	geom && BBox ( 106790, 56773, 17 ) 
	AND ST_Intersects ( geom, BBox ( 106790, 56773, 17 ) ) ​ 
	
	--string sql = "SELECT ST_AsMVT(tile, 'lines', 4096, 'geom') tile FROM(SELECT display, ST_AsMVTGeom(geom, ST_MakeEnvelope(:lonmin,:latmin,:lonmax,:latmax, 4326),4096, 256, true) AS geom FROM public.app_geo_graticule ) AS tile";
	--(float Left, float Top, float Right, float Bottom) bound = Util.CalculateBounds(x,y,z);
```



## 二、PostgreSQL相关笔记

### 1、常用工具箱SQL语句汇总

**杀掉空闲连接**

```sql
select * from pg_stat_activity where state='idle'
SELECT pg_cancel_backend('22508','27836');

关闭所有session:
SELECT pg_terminate_backend(pid) FROM pg_stat_activity WHERE datname=' 数据库名' AND pid<>pg_backend_pid(); 



WITH inactive_connections AS (
    SELECT
        pid,rank() over (partition by client_addr order by backend_start ASC) as rank
    FROM 
        pg_stat_activity
    WHERE
        -- Exclude the thread owned connection (ie no auto-kill)
        pid <> pg_backend_pid( )
    AND
        -- Exclude known applications connections
        application_name !~ '(?:psql)|(?:pgAdmin.+)'
    AND
        -- Include connections to the same database the thread is connected to
        datname = current_database() 
    AND
        -- Include connections using the same thread username connection
        usename = current_user 
    AND
        -- Include inactive connections only
        state in ('idle','idle in transaction','idle in transaction (aborted)','disabled') 
    AND
        -- Include old connections (found with the state_change field)
        current_timestamp - state_change > interval '5 minutes' 
)
SELECT
    pg_terminate_backend(pid)
FROM
    inactive_connections 
WHERE
    rank > 1 -- Leave one connection for each application connected to the database
		
```

**postgresql服务启动不了解决方法**

报错：

2018-12-05 14:06:50.130 HKT [4544] LOG: database system was shut down in recovery at 2018-12-05 14:04:29 HKT

2018-12-05 14:06:50.130 HKT [4544] LOG: invalid primary checkpoint record

2018-12-05 14:06:50.130 HKT [4544] LOG: invalid secondary checkpoint record

2018-12-05 14:06:50.130 HKT [4544] PANIC: could not locate a valid checkpoint record

2018-12-05 14:06:50.130 HKT [4348] 日志: 在恢复目标处关闭

2018-12-05 14:06:50.161 HKT [3760] LOG: shutting down

2018-12-05 14:06:50.270 HKT [4348] 日志: 数据库系统已关闭

解决方法：

pg_resetwal -f ..\data

**清理死会话**

select pg_terminate_backend(pid) from pg_stat_activity where state='idle';



**数据库表空间占用大小查询**

```sql
SELECT
	d.datname AS NAME,
	pg_catalog.pg_get_userbyid ( d.datdba ) AS OWNER,
CASE
		WHEN pg_catalog.has_database_privilege ( d.datname, 'CONNECT' ) THEN
		pg_catalog.pg_size_pretty (
		pg_catalog.pg_database_size ( d.datname )) ELSE'No Access' 
END AS SIZE 
FROM
	pg_catalog.pg_database d 
ORDER BY
CASE
		WHEN pg_catalog.has_database_privilege ( d.datname, 'CONNECT' ) THEN
	pg_catalog.pg_database_size ( d.datname ) ELSE NULL 
END DESC -- nulls first


--查看表
SELECT table_schema || '.' || table_name AS table_full_name, pg_total_relation_size('"' || table_schema || '"."' || table_name || '"')AS size
FROM information_schema.tables
ORDER BY
pg_total_relation_size('"' || table_schema || '"."' || table_name || '"') DESC
--查看表新
SELECT
  table_schema || '.' || table_name AS table_full_name,
  pg_size_pretty(total_size) AS total_size
FROM (
  SELECT
    table_schema,
    table_name,
    pg_total_relation_size(table_schema || '.' || table_name) AS total_size
  FROM information_schema.tables ORDER BY total_size DESC
) AS table_sizes
;


 SELECT pg_terminate_backend(pid) FROM pg_stat_activity WHERE datname='FlightHistoryDB' AND pid<>pg_backend_pid(); 
```

数据库查看表字段注释：

```sql
Select a.attnum,(select description from pg_catalog.pg_description where objoid=a.attrelid and objsubid=a.attnum) as descript ,a.attname,pg_catalog.format_type(a.atttypid,a.atttypmod) as data_type from pg_catalog.pg_attribute a where 1=1 and a.attrelid=(select oid from pg_class where relname='geo_img_dark' ) and a.attnum>0 and not a.attisdropped order by a.attnum;
--给字段添加中文注释
COMMENT ON COLUMN "public"."Untitled"."tile" IS 'ab';
```

关闭数据库所有连接：

```plsql
SELECT
	pg_terminate_backend ( pg_stat_activity.pid ) 
FROM
	pg_stat_activity 
WHERE
	pg_stat_activity.datname = 'test';
```

Postgresql 数据表结构查询语句

```sql
SELECT
	ns.nspname || '.' || C.relname AS ID,
	A.attname,
	T.typname,
CASE
		WHEN A.atttypmod > 0 
		AND A.atttypmod < 32767 THEN
			A.atttypmod - 4 ELSE A.attlen 
		END len,
CASE
	WHEN T.typelem = 0 THEN
	T.typname ELSE t2.typname 
	END,
CASE
	
	WHEN A.attnotnull THEN
	0 ELSE 1 
	END AS is_nullable,
	COALESCE (
		( SELECT 1 FROM pg_sequences WHERE sequencename = ns.nspname || '_' || C.relname || '_' || A.attname || '_sequence_name' LIMIT 1 ),
		0 
	) is_identity,
--e.adsrc as is_identity,
	d.description AS COMMENT,
	A.attndims,
CASE
		WHEN T.typelem = 0 THEN
		T.typtype ELSE t2.typtype 
	END,
	ns2.nspname,
	A.attnum 
FROM
	pg_class
	C INNER JOIN pg_attribute A ON A.attnum > 0 
	AND A.attrelid = C.oid
	INNER JOIN pg_type T ON T.oid = A.atttypid
	LEFT JOIN pg_type t2 ON t2.oid = T.typelem
	LEFT JOIN pg_description d ON d.objoid = A.attrelid 
	AND d.objsubid = A.attnum
	LEFT JOIN pg_attrdef e ON e.adrelid = A.attrelid 
	AND e.adnum = A.attnum
	INNER JOIN pg_namespace ns ON ns.oid = C.relnamespace
	INNER JOIN pg_namespace ns2 ON ns2.oid = T.typnamespace 
WHERE
	( ns.nspname || '.' || C.relname IN ( 'public.app_geo_point' ) ) 
	
	
	
	
	
```

**查询集群状态与表空间回收**

连上44库，执行 show pool_nodes; 语句可以看到集群各节点的瞬时状态，重点关注status和replication_delay，如果status不为up（如down等）或者replication_delay特别大且多次执行语句后值无变化则说明集群发生故障，联系我处理故障，后续我会把故障处理方法写个规范点的文档发给大家

```sql
--表空间清理SQL
TRUNCATE bigtable, fattable RESTART IDENTITY;
truncate table table_name;
alter sequence seq_name start 1;

TRUNCATE "TBHistory_FlightExtInfoHistory","TBHistory_FlightInfoHistory", "TBHistory_FlightInfoRealTime" RESTART IDENTITY;
Vacuum "TBHistory_FlightExtInfoHistory"


--需要归档sql
select t2.* from (SELECT t."HX",min(t."TE") as "ST" ,max(t."TE") as "ET" from "TBHistory_FlightInfoRealTime" t GROUP BY t."HX" order by max(t."TE")) t2 where t2."ET"<'2019-03-13 13:30:00' LIMIT 50
```







**数组操作**

```sql
select "F_Position"[2:300] from "TBBase_RealTimeFlightTrack" where "F_HX"='885175';
select "F_Position" from "TBBase_RealTimeFlightTrack" where "F_HX"='885175';
SELECT array_length("F_Position", 1) from "TBBase_RealTimeFlightTrack" where "F_HX"='885175';
select "F_Position"[array_length("F_Position", 1)-2:array_length("F_Position", 1)] from "TBBase_RealTimeFlightTrack" where "F_HX"='885175';
select "F_HX","F_Position"[array_length("F_Position", 1)-2:array_length("F_Position", 1)] from "TBBase_RealTimeFlightTrack" where "F_FN" like 'CCA%';


```

 **JSONB操作**

```sql
SELECT
  T2."Lat",
  T2."Lon",
  T2."FN",
  T2."F_Time"
FROM
	(
		SELECT
			T1."F_Time",
			jsonb_array_elements ("F_Content") ->> 'FN' AS "FN",
			jsonb_array_elements ("F_Content") ->> 'LA' AS "Lat",
			jsonb_array_elements ("F_Content") ->> 'LO' AS "Lon"
		FROM
			"TBHistory_Flights" T1
		WHERE
			T1."F_Time" > '2018-03-26 00:36:29'
	) AS T2
WHERE
T2."FN" = 'CCA4373'
-----------------------------------------------------------------------------------------------------JSONB数据字典操作SQL:
SELECT t.* from person t
--插入
INSERT INTO person (info) VALUES ('[{"value":1,"label":"摆渡车"},{"value":2,"label":"引导车"}]'::jsonb);
--增加
UPDATE person SET info = info || '[{"value":3,"label":"清洁车"}]'::jsonb;
--修改
UPDATE person t1 SET info = jsonb_set(info, array[(SELECT ORDINALITY::INT - 1 FROM person t2, jsonb_array_elements(info) WITH 
ORDINALITY WHERE t1.id = t2.id AND value->>'value' = '1')::text, 'label'::text], '"摆渡车更新4"') WHERE id = 8

--删除
update person set info = info - 0  where id = 8
--遍历
SELECT t.id, jsonb_array_elements(info) ->> 'value' AS "value" , jsonb_array_elements(info) ->> 'label' AS "label" FROM person t

--根据表t_map的old_id更新表t_1的t_id为表t_map的new_id
update t_1 t
set t_id = map.new_id
from t_map map
where t.t_id = map.old_id
and t.name = 'a'

----------------------------------------------------------------------------------------------
--jsonb-insert
update "TBApp_UserAppConfig_copy1" set "F_FlightStyleSet"=jsonb_insert("F_FlightStyleSet", '{flightLabelContentSet,19}', '{"Field": "AA","IsValid": false,"EndLabel": "","FontScale": 1,"IsNewline": false,"StartLabel": "","FieldRemark": "AA"}', true)

update "TBApp_UserFlightAlarmFilterRules_copy1" set "F_AlarmFilterContent"=jsonb_insert("F_AlarmFilterContent",'{alarmSet,19}','{"isIgnore":true,"isShowDlg":true,"isShowVoice":true,"showDlgType":"1","alarmTypeEnum":77,"alarmTypeName":"保护油","isAutoToPanel":true,"voiceTemplete":"保护油","alarmTypeColor":"#c1c1c1","alarmTypeLabel":"PRFU"}', true)




```

**聚合函数**

```sql
SELECT t.f_icao,t.f_iata,t.f_chinesef,string_agg(DISTINCT(t.fn), ',') from "adsbairports" t where t.he<3000 GROUP BY t.f_icao,t.f_iata,t.f_chinesef
```

**数据库备份与还原**

```sql
pg_dump -h localhost -U postgres DataCollect > F:\PGDbBack\DataCollect.bak 
psql -h localhost -U postgres -d DataCollect < C:\Users\Administrator\Desktop\PGBack\DataCollect.bak
pg_restore -h localhost -p 5432 -U postgres -W -d SDB_XYAirportForTime -v "D:\temp\SDB_XYAirportForTime.backup"
```

**创建自增队列**

```sql
create sequence "TBMap_AviationWeather_seq" increment by 1 minvalue 1 no maxvalue start with 1;  
```



------

**分区表相关**

**历史轨迹回放查询效率问题解决方法测试**

**解决方法**：新建数据库，把历史轨迹存储独立出来，并将与之相关的统计查询迁移过来。

**测试结果**：经测试，单车一小时的历史轨迹查询时长达到百毫秒级

 

1、表空间

表空间对应的物理存储，可理解为数据库的C盘、D盘等，所以数据库的所有对象都可以放进去，例如整个数据库、数据表、索引、函数等等。本测试将新建表空间，并更改整个库的默认表空间，未对单独表做表空间。

注意：涉及到终止数据库会话，所以查询最好在另一个数据库中执行。

```sql
create tablespace tbsgposition owner postgres location 'D:\GPosition\table_space'--创建表空间（需有对路径的读写权限）
select * from pg_stat_activity WHERE datname='GPositionDB'--查看数据库会话
SELECT pg_terminate_backend(6336)--根据pid终止数据库所有会话
alter database "GPositionDB" set tablespace tbsgposition--更改数据库默认表空间
select oid,datname,dattablespace from pg_database where datname='GPositionDB'--查看数据库当前表空间oid
select oid,* from pg_tablespace where oid=50834128--根据oid查看表空间名称，确认更改已完成
```

2、承实现分区表**

参考https://www.postgresql.org/docs/9.6/static/ddl-partitioning.html。

2.1建主表

针对历史轨迹表以时间作为检查项，不设主键，主表不建索引。参考如下：

```sql
CREATE TABLE "public"."VehicleHistory" (
"CarNum" text COLLATE "default",
"DateTime" timestamp(6),
"Position" "public"."geometry",
"Direction" float4,
"Speed" float4,
"CarType" int4,
"SN" text COLLATE "default",
"OS" bool,
"SS" bool,
"ES" int4,
"IA" bool,
"CD" text COLLATE "default",
"UD" text COLLATE "default",
"LT" timestamp(6),
"UN" text COLLATE "default",
"MS" bool,
"TE" timestamp(6),
"Content" json
)
```

2.2建子表

根据日期建子表继承自主表，并在关键字段上建索引。

```sql
CREATE TABLE "public"."VehicleHistory_20180303"(
  CHECK( "DateTime">=DATE '2018-03-03' AND "DateTime"<(DATE'2018-03-04'+INTERVAL'1 day'))
) INHERITS("VehicleHistory")
CREATE INDEX "index_vehicle_history_carnumber_20180303" ON "VehicleHistory_20180303" USING btree ("CarNum");
CREATE INDEX "index_vehicle_history_datetime_20180303" ON "VehicleHistory_20180303" USING btree ("DateTime");
CREATE INDEX "index_vehicle_history_cartype_20180303" ON "VehicleHistory_20180303" USING btree ("CarType");
```

2.3建触发器

建立触发器，当主表插入数据时，根据日期判断插入到对应子表中。

```plsql
CREATE OR REPLACE FUNCTION vehicle_history_insert_trigger()
RETURNS TRIGGER AS $$
BEGIN
​    IF ( NEW."DateTime" >= DATE '2018-03-02' AND NEW."DateTime" < DATE '2018-03-03' ) THEN
​    INSERT INTO "VehicleHistory_20180302" VALUES (NEW.*);
  ELSIF ( NEW."DateTime" >= DATE '2018-03-03' AND NEW."DateTime" < DATE '2018-03-04' ) THEN
​    INSERT INTO "VehicleHistory_20180303" VALUES (NEW.*);
​    ELSE
​    INSERT INTO "VehicleHistory_error_join_date" VALUES (NEW.*);
  END IF;
​    RETURN NULL;
END;
$$
LANGUAGE plpgsql;
CREATE TRIGGER insert_vehicle_history_trigger
BEFORE INSERT ON "VehicleHistory"
FOR EACH ROW EXECUTE PROCEDURE vehicle_history_insert_trigger();
**3****、定期更新**
子表的建立与触发器的更新可通过定期执行存储过程实现自动化。
CREATE OR REPLACE FUNCTION "public"."update_vehicle_partion_table"()
 RETURNS "pg_catalog"."bool" AS $BODY$
DECLARE
  currenttime TIMESTAMP;
  tbname TEXT;
  tbnametommorow TEXT;
  cnindex TEXT;
  dtindex TEXT;
  ctindex TEXT;
BEGIN
  currenttime:=current_date;
  tbname:='vehiclehistory'||current_date::TEXT;
  tbnametommorow:='vehiclehistory'||(current_date+INTERVAL'1 day')::DATE::TEXT;
  cnindex:='index_vehicle_history_carnumber_'||tbnametommorow;
  dtindex:='index_vehicle_history_datetime_'||tbnametommorow;
  ctindex:='index_vehicle_history_cartype_'||tbnametommorow;
  --每日凌晨执行一次，a、新建第二天的分区表；b、新建分区表索引；c、更新主表的触发器函数
  --a
  EXECUTE '
​    CREATE TABLE '||quote_ident(tbnametommorow)||'(
​      CHECK( "DateTime">=('||quote_literal(currenttime)||'::TIMESTAMP+INTERVAL''1 day'') AND "DateTime"<('||quote_literal(currenttime)||'::TIMESTAMP+INTERVAL''2 day''))
​    ) INHERITS("VehicleHistory")';
  --b
  EXECUTE format(
​    'CREATE INDEX %I ON %I USING btree ("CarNum")',
  cnindex,tbnametommorow);
  EXECUTE format(
​    'CREATE INDEX %I ON %I USING btree ("DateTime")',
  dtindex,tbnametommorow);
  EXECUTE format(
​    'CREATE INDEX %I ON %I USING btree ("CarType")',
  ctindex,tbnametommorow);
  --c
  EXECUTE 
​    'CREATE OR REPLACE FUNCTION vehicle_history_insert_trigger()RETURNS TRIGGER AS $tag$
​    BEGIN
​      IF ( NEW."DateTime" >= '||quote_literal(currenttime)||'::TIMESTAMP AND NEW."DateTime" < ('||quote_literal(currenttime)||'::TIMESTAMP+INTERVAL''1 day'') ) THEN
​        INSERT INTO '||quote_ident(tbname)||' VALUES (NEW.*);
​      ELSIF ( NEW."DateTime" >= ('||quote_literal(currenttime)||'::TIMESTAMP+INTERVAL''1 day'') AND NEW."DateTime" < ('||quote_literal(currenttime)||'::TIMESTAMP+INTERVAL''2 day'') ) THEN
​        INSERT INTO '||quote_ident(tbnametommorow)||' VALUES (NEW.*);
​      ELSE
​        INSERT INTO "VehicleHistory_error_join_date" VALUES (NEW.*);
​      END IF;
​      RETURN NULL;
​    END;
​    $tag$
​    LANGUAGE plpgsql';  
RETURN TRUE;
END;
$BODY$
 LANGUAGE 'plpgsql' VOLATILE COST 100
;
ALTER FUNCTION "public"."update_vehicle_partion_table"() OWNER TO "postgres";
```

 

2.4查询优化

当查询条件包括检查项——日期——时，可做如下设置

```sql
SET constraint_exclusion = on;
```

这样查询主表时，会优先筛选出相关日期子表再详细查询，可提高查询效率。

2.5其他

新版本的postgres对分区表的支持更好，效率也更优；另外还有一个扩展pg_pathman，效率更高也提供了更丰富的功能，但不支持widows平台。

------

 **Pathman分区表**

1. **所有语句在主库上执行**
2. **如果已经分区，删除分区语句示例：**

```sql
select drop_partitions('"TBHistory_FlightInfoRealTime"'::regclass, true);
select drop_partitions('"TBHistory_FlightExtInfoHistory"'::regclass, true);
select drop_partitions('"TBHistory_FlightInfoHistory"'::regclass, true);
select drop_partitions('" TBHistory_FlightExtInfoRealTime"'::regclass, true);
```



1. 按字段Hash分区示例：

 

```sql
select create_hash_partitions(
'"TBHistory_FlightInfoRealTime"'::regclass,        -- 主表oid
'"HX"',                -- 分区字段，一定要not null约束
50,                 -- 分区表数量
false                -- 不立即将数据从主表迁移到分区
);
select set_enable_parent('"TBHistory_FlightInfoRealTime"'::regclass, false);

select create_hash_partitions(
'"TBHistory_FlightExtInfoRealTime "'::regclass,        -- 主表oid
'"HX"',                -- 分区字段，一定要not null约束
5,                 -- 分区表数量
false                -- 不立即将数据从主表迁移到分区
);
select set_enable_parent('"TBHistory_FlightExtInfoRealTime"'::regclass, false);
```



1. 按时间字段分区示例：

```sql
select create_range_partitions(
'"TBHistory_FlightExtInfoHistory"'::regclass,         -- 主表oid
'"DT"',         -- 分区字段，一定要not null约束
'2019-01-01 00:00:00'::timestamp,  -- 开始时间
interval '1 day',1                -- 不立即将数据从主表迁移到分区
);
select set_enable_parent('"TBHistory_FlightExtInfoHistory"'::regclass, false);
select create_range_partitions(
'"TBHistory_FlightInfoHistory"'::regclass,         -- 主表oid
'"TE"',         -- 分区字段，一定要not null约束
'2019-01-01'::date,  -- 开始时间
interval '1 day',1                -- 不立即将数据从主表迁移到分区
);
select set_enable_parent('"TBHistory_FlightInfoHistory"'::regclass, false);
```



------

**upsert语句** 应用

--pg 9.5 版本支持 "UPSERT" 特性， 这个特性支持 INSERT 语句定义 ON CONFLICT DO UPDATE/IGNORE 属性，当插入 SQL 违反约束的情况下定义动作，而不抛出错误

 

--创建测试数据表

```plsql
create table t (id int constraint idx_t_id primary key,name varchar(20) constraint cst_name not null);
insert into t values(1,'rudy');

postgres=# select * from t;
 id | name 
----+------
 1 | rudy
(1 row)
```

 

--根据字段，当id冲突时更新name值

```plsql
postgres=# insert into t values(1,'rudy1') ON CONFLICT(id) do update set name=EXCLUDED.name ;

INSERT 0 1
postgres=# select * from t;
 id | name 
----+-------
 1 | rudy1
```

 

 

--也可以直接指定约束名，此时不需要字段，在实际应用中，最好使用字段名

```plsql
postgres=# insert into t values(2,'rudy3') ON CONFLICT ON CONSTRAINT idx_t_id do update set name=EXCLUDED.name ; 

INSERT 0 1
postgres=# select * from t;
 id | name 
----+-------
 1 | rudy1
 2 | rudy3
(2 rows)
```

 

 

--根据where条件选择性更新，由于id没有大于10的数据，故更新0条数据                                ^

```plsql
postgres=# insert into t values(2,'rudy4') ON CONFLICT ON CONSTRAINT idx_t_id do update set name=EXCLUDED.name where t.id>10 ;

INSERT 0 0
postgres=# select * from t;
 id | name 
----+-------
 1 | rudy1
 2 | rudy3
```

 

--只插入满足条件的数据行 

```plsql
postgres=# insert into t values(2,'rudy4'),(3,'rudy3') ON CONFLICT(id) do nothing ; 

INSERT 0 1
postgres=# select * from t;
 id | name 
----+-------
 1 | rudy1
 2 | rudy3
```

应用案例

```sql
insert into "TBBase_Test1" values ('cf1d6a6e-6077-4d3e-b8da-fe54ec6b20fa','a2','2018/11/5 16:29:53',0),('5d3af085-ca67-4794-9e56-52c5b7b32fc8','a-updated','2018/11/20 16:29:53',30) on conflict ON CONSTRAINT uk_tbbase_test1_name_age do update set "F_Name"=excluded."F_Name","F_UpdateTime"=excluded."F_UpdateTime","F_Age"=excluded."F_Age";
```

 

```C#
     /// <summary>
​    /// Upsert方法
​    /// </summary>
​    /// <param name="entities"></param>
​    /// <param name="strKeyFieldName"></param>
​    /// <returns></returns>
​    public static int Upsert(List<TBBase_Test1> entities, string strKeyFieldName)
​    {
​      if (entities.Count() == 0)
​        return 0;
​      List<string> lstSQLValue = new List<string>();
​      foreach (var item in entities)
​      {
​        lstSQLValue.Add(string.Format("('{0}','{1}','{2}',{3})", item.F_ID, item.F_Name, item.F_UpdateTime, item.F_Age));
​      }
​      string strSQLValues= string.Join(",",lstSQLValue.ToArray());
​      string strSQLUpdateSQL = string.Format("\"{0}\"=excluded.\"{0}\",\"{1}\"=excluded.\"{1}\",\"{2}\"=excluded.\"{2}\";", "F_Name", "F_UpdateTime", "F_Age");
​      string strDoSQL = string.Format("insert into \"{0}\" values {1} on conflict (\"{2}\") do update set {3};", "TBBase_Test1", strSQLValues, strKeyFieldName, strSQLUpdateSQL);
​      int count= Db.Context.FromSql(strDoSQL).ExecuteNonQuery();
​      return count;
​    }
```

 

 

```sql
 CONSTRAINT "TBBase_MissMatchInfo_pkey" PRIMARY KEY ("F_ID"),
 CONSTRAINT "Uniquekey_TBBase_MissmatchInfo" UNIQUE ("F_Value", "F_Type")

INSERT INTO "TBBase_MissmatchInfo"
VALUES('LKK',6,'a',uuid_generate_v4 ()) ON CONFLICT ON CONSTRAINT "Uniquekey_TBBase_MissmatchInfo"; --DO UPDATE SET "F_Desc" = EXCLUDED."F_Desc";

INSERT INTO "TBBase_MissmatchInfo"
VALUES('LKK',6,'',uuid_generate_v4 ()) ON CONFLICT ON CONSTRAINT "Uniquekey_TBBase_MissmatchInfo" DO

UPDATE 
SET "F_Desc" = case when EXCLUDED."F_Desc" = '' then "TBBase_MissmatchInfo"."F_Desc" else EXCLUDED."F_Desc" end;
```





## 三、业务相关SQL语句备份

### 1、ADSB覆盖度查询示例

```sql
Select t4."FN",t4."RE",t4."FT",t4."OA",t4."DA",t4."LO",t4."LA",t4."HE",t4."GV",t4."MT",'001' as "SR",t4."MTSpan" from (Select t2.*,t3."MT" as "MT2",t3."LO" as "LO2",t3."LA" as "LA2",extract(epoch FROM (t3."MT"-t2."MT")) as "MTSpan" from (SELECT row_number() over(ORDER BY t."TE") as "ID",t."HX",t."FN",t."RE",t."FT",t."LO",t."LA",t."OA",t."DA",t."TE",t."MT",T."SR",t."HE",t."GV" from "adsbsingaltable" t where t."FN"='CCA1351' and t."MT">'2018-08-05 07:40:00' and t."MT"<'2018-08-05 11:00:00' ORDER BY t."TE") t2
LEFT JOIN (SELECT row_number() over(ORDER BY t."TE")-1 as "ID",t."HX",t."FN",t."RE",t."LO",t."LA",t."OA",t."DA",t."TE",t."MT",T."SR",t."HE",t."GV" from "adsbsingaltable" t where t."FN"='CCA1351' and t."MT">'2018-08-05 07:40:00' and t."MT"<'2018-08-05 11:00:00' ORDER BY t."TE") t3
ON t2."ID"=t3."ID") t4 where t4."MTSpan"!=0 and (substr(t4."SR", 0, 4)='001' or substr(t4."SR", 0, 4)='002')
--间隔15秒
Select t4.* from (Select t2.*,t3."MT" as "MT2",t3."LO" as "LO2",t3."LA" as "LA2",extract(epoch FROM (t3."MT"-t2."MT")) as "MTSpan" from (SELECT row_number() over(ORDER BY t."TE") as "ID",t."HX",t."FN",t."RE",t."FT",t."LO",t."LA",t."OA",t."DA",t."TE",t."MT",T."SR",t."HE",t."GV" from "adsbsingaltable" t where t."FN"='CCA1351' and t."MT">'2018-08-05 07:40:00' and t."MT"<'2018-08-05 11:00:00' ORDER BY t."TE") t2
LEFT JOIN (SELECT row_number() over(ORDER BY t."TE")-1 as "ID",t."HX",t."FN",t."RE",t."LO",t."LA",t."OA",t."DA",t."TE",t."MT",T."SR",t."HE",t."GV" from "adsbsingaltable" t where t."FN"='CCA1351' and t."MT">'2018-08-05 07:40:00' and t."MT"<'2018-08-05 11:00:00' ORDER BY t."TE") t3
ON t2."ID"=t3."ID") t4 where t4."MTSpan"!=0 and (substr(t4."SR", 0, 4)='001' or substr(t4."SR", 0, 4)='002')
```

### 2、航班监控历史数据回查

```sql
explain analyze SELECT t.* from "TBHistory_FlightInfoRealTime" t WHERE t."FN"='CCA1306' ORDER BY t."TE"
SELECT t.* from "TBHistory_FlightInfoRealTime" t WHERE t."SR"='001_ZBDH001' LIMIT 1
SELECT t.* from "TBHistory_FlightInfoRealTime" t WHERE t."HX"='78058C' ORDER BY t."TE"
SELECT t.* from "TBHistory_FlightExtInfoHistory" t WHERE t."DT"='2019-12-04' AND t."FN2"='FU1106'
SELECT t.* from "TBHistory_FlightExtInfoHistory" t WHERE t."DT"='2019-12-04' AND t."HX"='780D03'
SELECT t.* from "TBHistory_FlightInfoHistory" t WHERE t."TE">'2019-12-05 00:00:08' AND  t."TE"<'2019-12-06 00:00:08'  AND t."HX"='78058C'  ORDER BY t."TE"
SELECT t.* from "TBHistory_FlightInfoHistory" t WHERE t."HX"='78058C'  LIMIT 1

SELECT row_number() over() as "F_ID",t.* from "FlightHistory" t where t."TE">'2018-07-25 00:00:00' and t."FL"='CCA' ORDER BY t."TE"
```

### 3、历史轨迹点抽吸

```sql
SELECT t."ID",t."HX",t."FN",substring(t."FN",0,4) as "FL",t."LA",t."LO",t."HE",substring(t."SR",0,4) as "SR",t."TE" from "TBHistory_FlightInfoHistory_318" t where mod(extract(second from t."TE")::int,5)=0

SELECT t."ID",t."HX",t."FN",substring(t."FN",0,4) as "FL",t."LA",t."LO",t."HE",substring(t."SR",0,4) as "SR",t."TE" from "TBHistory_FlightInfoHistory" t WHERE t."TE">'2021-06-17' and t."TE"<'2021-06-22' and mod(extract(second from t."TE")::int,30)=0 --LIMIT 10

```

### 4、航班动态生产库冷热备份

```
*操作服务器：172.30.7.139  administrator Cast@2019-casT
变更为：172.30.5.50 Jiankong@2021-ZY

冷库连接参数：172.30.2.95 postgres：postgres
root Cast@2020

--服务器参数
操作服务器：172.30.7.139  administrator Cast@2019-casT
冷库连接参数：172.30.2.95 postgres：postgres
root Cast@2020
----热库查询分区情况
SELECT
	t2.* 
FROM
	( SELECT parent :: VARCHAR,"partition"::VARCHAR,range_min,range_max FROM pathman_partition_list ) t2 
WHERE
	t2.parent = '"TBHistory_FlightInfoHistory"'
	and t2.range_max<='2020-01-01 00:00:00'
	ORDER BY t2.range_max
--冷库查询分区情况
SELECT
	t2.* 
FROM
	( SELECT parent :: VARCHAR,"partition"::VARCHAR,range_min,range_max FROM pathman_partition_list ) t2 
WHERE
	t2.parent = 'flightinfohistory'
	and t2.range_max<='2020-01-01 00:00:00'
	ORDER BY t2.range_max DESC
	LIMIT 5


```

查看分区表情况SQL

```sql
--历史轨迹库备份最终
--热库查询分区情况
SELECT
	t2.* 
FROM
	( SELECT parent :: VARCHAR,"partition"::VARCHAR,range_min,range_max FROM pathman_partition_list ) t2 
WHERE
	t2.parent = '"TBHistory_FlightInfoHistory"'
	and t2.range_max<='2020-01-01 00:00:00'
	ORDER BY t2.range_max
--冷库查询分区情况
SELECT
	t2.* 
FROM
	( SELECT parent :: VARCHAR,"partition"::VARCHAR,range_min,range_max FROM pathman_partition_list ) t2 
WHERE
	t2.parent = 'flightinfohistory'
	and t2.range_max<='2020-01-01 00:00:00'
	ORDER BY t2.range_max DESC
	LIMIT 5
	
	
生成热备SQL语句：归档冷库

drop schema history cascade;  
create schema history;
import foreign schema public from server hisdb into history;



drop schema analysis cascade;  
create schema analysis;
import foreign schema public from server analysisdb into analysis;





历史轨迹库：
	Select 'insert into flightextinfohistory(hx,fl,dt,fn,fn2,ft,re,oa,da,cid,fid,cs,cf,rs,fsu,apt,std,eta,oft,ont,dot,dct,st,et) select "HX" hx,"FL" fl,"DT" dt,"FN" fn,"FN2" fn2,"FT" ft,"RE" re,"OA" oa,"DA" da,"CID" cid,"FID" fid,"CS" cs,"CF" cf,"RS" rs,"FSU" fsu,"APT" apt,"STD" std,"ETA" eta,"OFT" oft,"ONT" ont,"DOT" dot,"DCT" dct,"ST" st,"ET" et from history.'||t3."partition"||';select dblink(''dbname=FlightHistoryDB host=172.30.2.92 port=5432 user=historyuser password=cast2019'',''TRUNCATE table '||t3."partition"||''');select dblink(''dbname=FlightHistoryDB host=172.30.2.92 port=5432 user=historyuser password=cast2019'',''drop table '||t3."partition"||''');'  from (SELECT
	t2.* 
FROM
	( SELECT parent :: VARCHAR,"partition"::VARCHAR,range_min,range_max FROM pathman_partition_list ) t2 
WHERE
	t2.parent = '"TBHistory_FlightExtInfoHistory"'
	and t2.range_max<='2021-02-20 00:00:00'
	ORDER BY t2.range_max) t3
	
	
	SELECT 'insert into flightinfohistory(hx,fn,la,lo,co,he,gv,vs,sc,sr,sl,lt,al,ls,fs,te) select "HX" hx,"FN" fn,"LA" la,"LO" lo,"CO" co,"HE" he,"GV" gv,"VS" vs,"SC" sc,"SR" sr,"SL" sl,"LT" lt,"AL" al,"LS" ls,"FS" fs,"TE" te from history.'||t3."partition"||';select dblink(''dbname=FlightHistoryDB host=172.30.2.92 port=5432 user=historyuser password=cast2019'',''TRUNCATE table '||t3."partition"||''');select dblink(''dbname=FlightHistoryDB host=172.30.2.92 port=5432 user=historyuser password=cast2019'',''drop table '||t3."partition"||''');' from (SELECT
	t2.* 
FROM
	( SELECT parent :: VARCHAR,"partition"::VARCHAR,range_min,range_max FROM pathman_partition_list ) t2 
WHERE
	t2.parent = '"TBHistory_FlightInfoHistory"'
	and t2.range_max<='2021-02-20 00:00:00'
	ORDER BY t2.range_max) t3	
	

	
	

直接删除SQL:
分析库：
		Select 'TRUNCATE table '||t3."partition"||';' || 'drop table '||t3."partition"||';' from (SELECT
	t2.* 
FROM
	( SELECT parent :: VARCHAR,"partition"::VARCHAR,range_min,range_max FROM pathman_partition_list ) t2 
WHERE
	t2.parent in ('"TBAnalysis_FlowCompute"','"TBAnalysis_FlowCompute"','"TBAnalysis_AirportMode"','"TBAnalysis_RwyCompute"')
	and t2.range_max<='2021-02-20 00:00:00'
	ORDER BY t2.range_max) t3  
	
	
气象库：
SELECT 'TRUNCATE table '||t3."partition"||';' ||'drop table '||t3."partition"||';' from (SELECT
	t2.* 
FROM
	( SELECT parent :: VARCHAR,"partition"::VARCHAR,range_min,range_max FROM pathman_partition_list ) t2 
WHERE
	t2.parent in ('"TBWeather_MetarSpeciInfo"','"TBWeather_SVCloudImage"','"TBWeather_SVRadarImage"','"TBWeather_WeatherImage"')
	and t2.range_max<='2021-02-20 00:00:00'
	ORDER BY t2.range_max) t3

	
	
```

0、建冷数据库的通用操作，本文档所有操作均在冷数据库中执行

```plsql
create extension postgis;

create extension postgres_fdw;

create extension pg_pathman;

create server hisdb foreign data wrapper postgres_fdw options(host '172.30.2.92' ,port '5432' ,dbname '"FlightHistoryDB"');

create user mapping for postgres server hisdb options(user 'historyuser',password 'cast2019');

//import foreign schema操作是一次性导入当前时刻的外部表，随着时间的推移热库里会创建新的分区表，但冷库看不到新增的分区表即冷库中的外部表不会自动更新，迁移到最后一张表之后再无新的外部表，此时可通过如下操作删除该schema、重新导入外部表，类似刷新操作
drop schema history cascade;  

create schema history;

import foreign schema public from server hisdb into history;

alter server hisdb options(set dbname 'FlightHistoryDB');

alter server hisdb options(set host '172.30.2.90');

//设置一次迁移多少条记录
alter server hisdb options(set fetch_size '250000');
```







1、FlightHistoryDB 
       1.1、fdw创建配置

```sql
    create server hisdb foreign data wrapper postgres_fdw options(host '172.30.2.92' ,port '5432' ,dbname '"FlightHistoryDB"');
    create user mapping for postgres server hisdb options(user 'historyuser',password 'cast2019');
    create schema history;
    import foreign schema public from server hisdb into history;
    alter server hisdb options(set fetch_size '250000');
```

1.2、迁移表 TBHistory_FlightExtInfoHistory
    创建本地表，不需再执行，供参考
    

```sql
CREATE TABLE "public"."flightextinfohistory" (
    "id" int8 DEFAULT nextval('flightextinfohistory_id_seq'::regclass) NOT NULL,
    "hx" varchar(20) COLLATE "default" NOT NULL,
    "fl" varchar(10) COLLATE "default",
    "dt" date NOT NULL,
    "fn" varchar(20) COLLATE "default",
    "fn2" varchar(20) COLLATE "default",
    "ft" varchar(20) COLLATE "default",
    "re" varchar(10) COLLATE "default",
    "oa" varchar(10) COLLATE "default",
    "da" varchar(10) COLLATE "default",
    "cid" varchar(50) COLLATE "default",
    "fid" varchar(50) COLLATE "default",
    "cs" varchar(30) COLLATE "default",
    "cf" varchar(30) COLLATE "default",
    "rs" varchar(20) COLLATE "default",
    "fsu" varchar(30) COLLATE "default",
    "apt" varchar(20) COLLATE "default",
    "std" timestamp(6),
    "eta" timestamp(6),
    "oft" timestamp(6),
    "ont" timestamp(6),
    "dot" timestamp(6),
    "dct" timestamp(6),
    "st" timestamp(6),
    "et" timestamp(6),
    CONSTRAINT "flightextinfohistory_pkey" PRIMARY KEY ("id")
    )
```



```sql
--迁移数据，注意待迁移的表名和他的分区时间范围
insert into flightextinfohistory(hx,fl,dt,fn,fn2,ft,re,oa,da,cid,fid,cs,cf,rs,fsu,apt,std,eta,oft,ont,dot,dct,st,et) 
select "HX" hx,"FL" fl,"DT" dt,"FN" fn,"FN2" fn2,"FT" ft,"RE" re,"OA" oa,"DA" da,"CID" cid,"FID" fid,"CS" cs,"CF" cf,"RS" rs,"FSU" fsu,"APT" apt,"STD" std,"ETA" eta,"OFT" oft,"ONT" ont,"DOT" dot,"DCT" dct,"ST" st,"ET" et 
from history."TBHistory_FlightExtInfoHistory_49";

//迁移成功后删除热库中的子表，确认迁移成功再删除，慎重
select dblink('dbname=FlightHistoryDB host=172.30.2.92 port=5432 user=historyuser password=cast2019','drop table "TBHistory_FlightExtInfoHistory_49"')
```

1.3、迁移表 TBHistory_FlightInfoHistory
    //创建本地表，不需再执行，供参考

```sql
 CREATE TABLE "public"."flightinfohistory" (
    "id" int8 DEFAULT nextval('flightinfohistory_id_seq'::regclass) NOT NULL,
    "hx" varchar(20) COLLATE "default" NOT NULL,
    "fn" varchar(20) COLLATE "default",
    "la" float8,
    "lo" float8,
    "co" int4,
    "he" int4,
    "gv" int4,
    "vs" int4,
    "sc" varchar(10) COLLATE "default",
    "sr" varchar(20) COLLATE "default",
    "sl" varchar(10) COLLATE "default",
    "lt" int4,
    "al" bool,
    "ls" int4[],
    "fs" int4,
    "te" timestamp(6) NOT NULL,
    CONSTRAINT "flightinfohistory_pkey" PRIMARY KEY ("id")
    )
```



```sql
CREATE INDEX "idx_flightinfohistory_fn" ON "public"."flightinfohistory" USING btree ("fn");

CREATE INDEX "idx_flightinfohistory_hx" ON "public"."flightinfohistory" USING btree ("hx");

select create_range_partitions('flightinfohistory','te','2019-07-11'::date,'1 day'::interval);
```


```sql
//迁移数据，注意待迁移的表名和他的分区时间范围
insert into flightinfohistory(hx,fn,la,lo,co,he,gv,vs,sc,sr,sl,lt,al,ls,fs,te) select "HX" hx,"FN" fn,"LA" la,"LO" lo,"CO" co,"HE" he,"GV" gv,"VS" vs,"SC" sc,"SR" sr,"SL" sl,"LT" lt,"AL" al,"LS" ls,"FS" fs,"TE" te from history."TBHistory_FlightInfoHistory_49";

//迁移成功后删除热库中的子表，确认迁移成功再删除，慎重
select dblink('dbname=FlightHistoryDB host=172.30.2.92 port=5432 user=historyuser password=cast2019','drop table "TBHistory_FlightInfoHistory_49"')
```



2、FlightAnalysisInfoDB
      2.1、fdw创建配置
    

```sql
create server analysisdb foreign data wrapper postgres_fdw options(host '172.30.2.92' ,port '5432' ,dbname 'FlightAnalysisInfoDB');
    create user mapping for postgres server analysisdb options(user 'analysisinfouser',password 'cast2019');
    create schema analysis;
    import foreign schema public from server analysisdb into analysis;
    alter server analysisdb options(fetch_size '250000');   //第一次设置时
    alter server analysisdb options(set fetch_size '250000');   第二次及以后设置时
```

2.2、迁移表 TBAnalysis_FlowCompute  创建本地表，不需再执行，供参考

```sql
create sequence flowcompute_id_seq;
CREATE TABLE "public"."flowcompute" (
"id" int8 DEFAULT nextval('flowcompute_id_seq'::regclass) NOT NULL,
"fn" varchar(20) COLLATE "default",
"oa" varchar(8) COLLATE "default",
"da" varchar(8) COLLATE "default",
"he" int4,
"co" int4,
"gridid" varchar(20) COLLATE "default",
"te" timestamp(6) NOT NULL,
"re" varchar(20) COLLATE "default",
CONSTRAINT "flowcompute_pkey" PRIMARY KEY ("id")
)

CREATE INDEX "idx_flowcompute_te" ON "public"."flowcompute" USING btree ("te");

CREATE INDEX "idx_flowcompute_grid" ON "public"."flowcompute" USING hash ("gridid");

insert into flowcompute(fn,oa,da,he,co,gridid,te,re) 
select "F_FN" fn,"F_OA" oa,"F_DA" da,"F_HE" he,"F_CO" co,"F_GridID" gridid,"F_Time" te,"F_RE" re
from analysis."TBAnalysis_FlowCompute_100" limit 1;

select create_range_partitions('flowcompute','te','2019-12-06'::date,'1 day'::interval);
```

2.3、迁移表 TBAnalysis_AirportMode
    //创建本地表，不需再执行，供参考

```sql
 create sequence airportmode_id_seq;
```



```sql
CREATE TABLE "public"."airportmode" (
"id" int8 DEFAULT nextval('airportmode_id_seq'::regclass),
"direction" varchar(5) COLLATE "default",
"fmode" varchar(10) COLLATE "default",
"down" varchar(16) COLLATE "default",
"up" varchar(16) COLLATE "default",
"icao" varchar(4) COLLATE "default",
"iata" varchar(4) COLLATE "default",
"te" timestamp(6) NOT NULL
)

CREATE INDEX "idx_airportMode_iata" ON "public"."airportmode" USING hash ("iata");

CREATE INDEX "idx_airportMode_icao" ON "public"."airportmode" USING hash ("icao");

CREATE INDEX "idx_airportMode_time" ON "public"."airportmode" USING btree ("te");

insert into airportmode(direction,fmode,down,up,icao,iata,te) 
select "F_Direction" direction,"F_Mode" fmode,"F_Down" down,"F_Up" up,"F_ICao" icao,"F_Iata" iata,"F_Time" te
from analysis."TBAnalysis_AirportMode_1" limit 1;

select create_range_partitions('airportmode','te','2019-08-26'::date,'7 day'::interval);



//迁移数据，注意待迁移的表名和他的分区时间范围
insert into airportmode(direction,fmode,down,up,icao,iata,te) 
select "F_Direction" direction,"F_Mode" fmode,"F_Down" down,"F_Up" up,"F_ICao" icao,"F_Iata" iata,"F_Time" te
from analysis."TBAnalysis_AirportMode_1";

//迁移成功后删除热库中的子表，确认迁移成功再删除，慎重
select dblink('dbname=FlightAnalysisInfoDB host=172.30.2.92 port=5432 user=analysisinfouser password=cast2019','drop table "TBAnalysis_AirportMode_1"');
```

2.4、迁移表 TBAnalysis_RwyCompute
    //创建本地表，不需再执行，供参考

```sql
create sequence rwycompute_id_seq;
CREATE TABLE "public"."rwycompute" (
"id" int8 DEFAULT nextval('rwycompute_id_seq'::regclass),
"fn" varchar(12) COLLATE "default",
"re" varchar(12) COLLATE "default",
"oa" varchar(8) COLLATE "default",
"da" varchar(8) COLLATE "default",
"downup" varchar(4) COLLATE "default",
"rwynode" varchar(4) COLLATE "default",
"rwy" varchar(8) COLLATE "default",
"icao" varchar(4) COLLATE "default",
"iata" varchar(4) COLLATE "default",
"te" timestamp(6) NOT NULL
)

CREATE INDEX "idx_rwycompute_iata" ON "public"."rwycompute" USING hash ("iata");

CREATE INDEX "idx_rwycompute_icao" ON "public"."rwycompute" USING hash ("icao");

CREATE INDEX "idx_rwycompute_time" ON "public"."rwycompute" USING btree ("te");

insert into rwycompute(fn,re,oa,da,downup,rwynode,rwy,icao,iata,te) 
select "F_FN" fn,"F_RE" re,"F_OA" oa,"F_DA" da,"F_DownUp" downup,"F_RwyNode" rwynode,"F_Rwy" rwy,"F_ICao" icao,"F_Iata" iata,"F_Time" te
from analysis."TBAnalysis_RwyCompute_1" limit 1;

select create_range_partitions('rwycompute','te','2019-08-26'::date,'7 day'::interval);

//迁移数据，注意待迁移的表名和他的分区时间范围
insert into rwycompute(fn,re,oa,da,downup,rwynode,rwy,icao,iata,te) 
select "F_FN" fn,"F_RE" re,"F_OA" oa,"F_DA" da,"F_DownUp" downup,"F_RwyNode" rwynode,"F_Rwy" rwy,"F_ICao" icao,"F_Iata" iata,"F_Time" te
from analysis."TBAnalysis_RwyCompute_1"

//迁移成功后删除热库中的子表，确认迁移成功再删除，慎重
select dblink('dbname=FlightAnalysisInfoDB host=172.30.2.92 port=5432 user=analysisinfouser password=cast2019','drop table "TBAnalysis_RwyCompute_1"')
```


3、FlightWeatherDB

```sql
create server weatherdb foreign data wrapper postgres_fdw options(host '172.30.2.92' ,port '5432' ,dbname 'FlightWeatherDB');
create user mapping for postgres server weatherdb options(user 'weatheruser',password 'cast2019');
create schema weather;
import foreign schema public from server weatherdb into weather;
alter server weatherdb options(fetch_size '250000');

TBWeather_MetarSpeciInfo
TBWeather_SVCloudImage
TBWeather_SVRadarImage
TBWeather_WeatherImage

--热裤执行
drop table "TBWeather_MetarSpeciInfo_1"
drop table "TBWeather_SVCloudImage_1"
drop table "TBWeather_SVRadarImage_1"
drop table "TBWeather_WeatherImage_1"
```

```sql
--基本飞机库查重
SELECT t2.* from (SELECT t."F_Hex",t."F_Reg",length(t."F_Hex") as "Len" from "TBBase_AircraftInfo" t) t2 where t2."Len">6
```

4、机队信息维护SQL

```sql
1、先删除已有的HX:DELETE from "TBBase_AircraftInfo" t where t."F_Hex" in ('781400','7813FF')
2、按格式对应插入SQL:
INSERT INTO "TBBase_AircraftInfo"("F_Hex", "F_Reg", "F_AircraftMode", "F_AirlinesCodes", "F_CompanyBranch", "F_AircraftType", "F_AirlinesName") VALUES ('7813C0', 'B1092', 'AIRBUSA321-271N', 'CSN', 'FlightRadar24', 'A21N', 'China Southern Airlines');


```

![image-20200501164126668](E:\源代码记录\Typora_mds\Postgresql笔记记录.assets\image-20200501164126668.png)

## 四、PostgreSQL相关资料汇总

1、航旅sqlite日志查询sql语句

```
select t2.* from (select Timestamp
, json_extract(t.value, "$.HX") as HX
,json_extract(t.value,"$.FN") as FN
,json_extract(t.value,"$.FN2") as FN2
,json_extract(t.value,"$.RE") as RE
,json_extract(t.value,"$.LO") as LO
,json_extract(t.value,"$.LA") as LA
,json_extract(t.value,"$.HE") as HE
,json_extract(t.value,"$.TE") as TE
from Logs,json_each(RenderedMessage) t) t2 where t2.HX='781690' 


-dblink;
create extension dblink;

INSERT INTO "TBHistory_FlightInfoHistory0708" select * from dblink('dbname=FlightHistoryDB host=172.30.2.92 port=5432 user=postgres password=postgres','select "ID", "HX", "FN", "LA", "LO", "CO", "HE", "GV", "VS", "SC", "TE" from "TBHistory_FlightInfoHistory_548" LIMIT 10' ) AS testTable ("ID" int8,
  "HX" varchar(20) COLLATE "pg_catalog"."default",
  "FN" varchar(20) COLLATE "pg_catalog"."default",
  "LA" float8,
  "LO" float8,
  "CO" int4,
  "HE" int4,
  "GV" int4,
  "VS" int4,
  "SC" varchar(10) COLLATE "pg_catalog"."default",
  "TE" timestamp(6));
```

## 五、常用业务复杂的SQL

```SQL
SELECT distinct on (t.flt_id)t.flt_id,t.flt_key,t.flt_reg,t.call_sign_icao from app_task_node t WHERE t.update_time>'2021-03-12' AND t.update_time<='2021-03-25' --GROUP BY t.flt_id
--distinct on 为Postgresql特有语法，重复取第一条


SELECT t2.flt_id,json_agg('{"task_code":'|| t2.task_code ||',"task_min_node":'||t2.min_node||',"task_min_node_time":"'||t2.min_time||'","task_max_node":'||t2.max_node||',"task_max_node_time":"'||t2.max_time||'"}') from (SELECT t.flt_id,t.task_code,min(t.task_node) as min_node,max(t.task_node) as max_node,min(t.submit_time) as min_time,max(t.submit_time) as max_time from app_task_node t WHERE t.update_time>'2021-03-12' AND t.update_time<='2021-03-25' and t.task_node!=21  GROUP BY t.flt_id,t.task_code ORDER BY t.task_code) t2 GROUP BY t2.flt_id





WITH flt_info AS (
 SELECT t2.* from (SELECT distinct on (t.flt_id)t.flt_id,t.flt_key,t.flt_reg,t.call_sign_icao from app_task_node t WHERE t.update_time>'2021-03-12' AND t.update_time<='2021-03-25') t2 LIMIT 10 OFFSET 0
   ),task_node_info AS (
SELECT t2.flt_id,json_agg('{"task_code":'|| t2.task_code ||',"task_min_node":'||t2.min_node||',"task_min_node_time":"'||t2.min_time||'","task_max_node":'||t2.max_node||',"task_max_node_time":"'||t2.max_time||'"}') as task_node_info from (SELECT t.flt_id,t.task_code,min(t.task_node) as min_node,max(t.task_node) as max_node,min(t.submit_time) as min_time,max(t.submit_time) as max_time from app_task_node t WHERE t.update_time>'2021-03-12' AND t.update_time<='2021-03-25' and t.task_node!=21  GROUP BY t.flt_id,t.task_code ORDER BY t.task_code) t2 GROUP BY t2.flt_id

   )
SELECT flt_info.*,task_node_info.task_node_info
FROM   flt_info LEFT JOIN task_node_info ON flt_info.flt_id=task_node_info.flt_id;

--非常准历史数据导入
SELECT '780C11' as "HX",t.fnum as "FN",t.latitude as "LA",t.longitude as "LO",t.angle as "CO",t.height as "HE",t.speed as "GV",0 as "VS",'6433' as "SC",'002' as "SR",'02' as "SL",0 as "LT",false as "AL", null as "LS",4 as "FS", to_char(to_timestamp(t.updatetime) AT TIME ZONE 'UTC-8','yyyy-MM-dd HH24:MI:SS') as "TE",to_timestamp(t."UTC Time",'yyyy-MM-dd hh24:mi:ss') as "TE",to_timestamp(t.updatetime) from "Variflight_RLH5351_20210426" t




INSERT INTO "public"."TBHistory_FlightExtInfoHistory"("HX", "FL", "DT", "FN", "FN2", "FT", "RE", "OA", "DA", "CID", "FID", "CS", "CF", "RS", "FSU", "APT", "STD", "ETA", "OFT", "ONT", "DOT", "DCT", "ST", "ET") VALUES ('780C11', 'RLH', '2021-04-26', 'RLH5351', 'DR5351', 'B737', 'B5812', 'WNH', 'CSX', '20210426_DR5351_WNH_CSX', '3d7eee65-b2ee-42ea-b075-ece540208be2', '----', '----', '', '----', '----', NULL, NULL, NULL, NULL, NULL, NULL, '2021-04-26 09:25:00', '2021-04-26 10:10:53');

```

## 六、Citus

```sql
create extension citus;  

CREATE DATABASE "db_demo"
WITH
  OWNER = "postgres"
  TEMPLATE = "template0"
  TABLESPACE = "pg_default"
;

SELECT * from master_add_node('172.30.7.174', 5432);
SELECT * from master_add_node('172.30.7.175', 5432);


SELECT * FROM master_get_active_worker_nodes();


create table test_table(id int, name varchar(16));
SELECT master_create_distributed_table('test_table', 'id', 'hash');
SELECT master_create_worker_shards('test_table', 4, 2);
select tablename from pg_tables where schemaname='public';



insert into ch_table_no_citus SELECT generate_series(1,1000000) as id,repeat( chr(int4(random()*26)+65),4) as name
insert into lhh_table SELECT generate_series(1,1000000) as id,repeat( chr(int4(random()*26)+65),4) as name

SELECT t.* from lhh_table t WHERE id<50000

SELECT t.* from ch_table_no_citus t WHERE id<50000
```

## 七、常用SQL

```sql
SELECT t.*,to_char(to_timestamp(t."TE"/1000),'YYYY-MM-DD HH24:MI:SS') as "TIME" from "3动态飞机20210705debug" t WHERE t."HX"='7BB133' and t."HE"<200 ORDER BY t."TE"


SELECT t.*,to_char(to_timestamp(t."TE"/1000),'YYYY-MM-DD HH24:MI:SS') as "TIME" from "3动态飞机20210705_2debug" t WHERE t."HX"='7BB133' and t."HE"<200 ORDER BY t."TE"

--表空间位置查看
select *,pg_tablespace_location(oid) from pg_tablespace;
--新建表空间 删除表空间
create tablespace tbs_db_base_info location '/home/data/tblspc';
drop tablespace if exists tbs_db_base_info;
--修改数据库名称 指定表空间
alter database ccm_db_airport rename to flm_db_map_airport;
alter database flm_db_map_airport set tablespace tbs_db_map_airport;
--关闭数据库所有连接
select pg_terminate_backend(pid) from (select pid from pg_stat_activity where datname = 'ccm_db_airport' ) a;
--按模板创建数据库
create database mydb1 template mydb_template;

--关联后 生成Json数组，父子关系
With tempTaskArrange AS
(	SELECT t3.* from (select t1.*,
	    (
      select array_to_json(array_agg(row_to_json(d)))
      from (
        select t2.task_id as "TaskID", t2.task_code,t2.task_sub_type
        from app_task_arrange t2
        where t2.flt_id=t1.flt_id and t2.task_sub_type=1
        order by t2.task_code asc
      ) d
    ) as "FltTasks"
	 from  app_task_flt t1
	 where t1.flt_key='CZ-3115-20210521160000-D') t3)
	 Select t4.flt_stda as "flt_af_stda",t4.flt_etda as "flt_af_etda",t4.flt_atda as "flt_af_atda",tempTaskArrange.* From tempTaskArrange
	 LEFT JOIN app_task_flt t4
	 ON tempTaskArrange.flt_af_id=t4.flt_id
	 WHERE json_array_length(tempTaskArrange."FltTasks")<>0
	 --WHERE tempTaskArrange."FltTasks" is not null
	 
	 
	 
	 
	 
 public async Task<(long, List<TaskArrangeGroupReturnDto>)> GetTaskArrangesGroupAsync2(TaskArrangeGroupParam taskArrangeGroupParam)
        {

            string sqlWith = @"SELECT t3.* from (select t1.*,
	    (
      select array_to_json(array_agg(row_to_json(d)))
      from (
        select t2.task_id, t2.task_code,t2.task_sub_type
        from app_task_arrange t2
        where t2.flt_id=t1.flt_id and t2.task_sub_type=1
        order by t2.task_code asc
      ) d
    ) as flt_tasks
	 from  app_task_flt t1
	 where t1.flt_key like '%CZ-3%' and t1.excute_date='2021-10-22')  t3";

            
           
            
            With a AS
(	SELECT t3.* from (select t1.flt_id,
	    (
      select count(task_id)
      from (
        select t2.task_id
        from app_task_arrange t2
        where t2.flt_id=t1.flt_id and t2.task_sub_type=1
        order by t2.task_code asc
      ) d
    ) as flt_tasks
	 from  app_task_flt t1
	 where t1.flt_key like '%CZ-3%' and t1.excute_date='2021-10-22') t3)
	 Select count(a.flt_id) From a
	 WHERE a.flt_tasks<>0

            
            
            
            
            
 public class TaskArrangeGroupReturnTest
    {
        [Column(RereadSql = "a.flt_id")]
        public long FltId { get; set; }
        [Column(RereadSql = "a.flt_key")]
        public string FltKey { get; set; }
        [Column(RereadSql = "b.flt_stda")]
        public DateTime FltAfStda { get; set; }
        [Column(RereadSql = "a.flt_tasks")]
        public JToken FltTasks { get; set; }
    }     
            
var queryEnd1= DbSet.DbAppFreeSql.Select<object>()
     .WithSql(sqlWith)
     .LeftJoin("app_task_flt b ON a.flt_af_id=b.flt_id")
    .WhereIf(true, "json_array_length(a.flt_tasks)<>0")
    .Page(1, 10)
    .ToList(a => new { FltId = "a.flt_id", FltKey = "a.flt_key", FltAfStda = "b.flt_stda", FltTasks = "a.flt_tasks" });


            string rq = DbSet.DbAppFreeSql.Select<TaskArrangeGroupReturnTest>()
      .WithSql(sqlWith)
      .LeftJoin("app_task_flt b ON a.flt_af_id=b.flt_id")
     .WhereIf(true, "json_array_length(a.flt_tasks)<>0")
     .Page(1, 10)
     .ToSql();

            List<TaskArrangeGroupReturnTest> r = DbSet.DbAppFreeSql.Select<TaskArrangeGroupReturnTest>()
          .WithSql(sqlWith)
          .LeftJoin("app_task_flt b ON a.flt_af_id=b.flt_id")
         .WhereIf(true, "json_array_length(a.flt_tasks)<>0")
         .Page(1, 10)
         .ToList<TaskArrangeGroupReturnTest>();




            //.ToList(a => new {
            //  bid = Convert.ToInt32("b.Id"),
            //  bName = "b.Name"
            // });
            //.ToList<object>("*");
            //.ToSql();



            return (0,null);
        }
```

```
	随机数据SQL
	
	Select t3.table_full_name,t3."size"/1024/1024 || 'M' as size from (SELECT table_schema || '.' || table_name AS table_full_name, pg_total_relation_size('"' || table_schema || '"."' || table_name || '"')AS size
FROM information_schema.tables
ORDER BY
pg_total_relation_size('"' || table_schema || '"."' || table_name || '"') DESC) t3
WHERE t3.table_full_name='public.tb_test_copy1'

	Select t3.table_full_name,t3."size"/1024/1024 || 'M' as size from (SELECT table_schema || '.' || table_name AS table_full_name, pg_total_relation_size('"' || table_schema || '"."' || table_name || '"')AS size
FROM information_schema.tables
ORDER BY
pg_total_relation_size('"' || table_schema || '"."' || table_name || '"') DESC) t3
WHERE t3.table_full_name='public.tb_test_copy2'

	Select t3.table_full_name,t3."size"/1024/1024 || 'M' as size from (SELECT table_schema || '.' || table_name AS table_full_name, pg_total_relation_size('"' || table_schema || '"."' || table_name || '"')AS size
FROM information_schema.tables
ORDER BY
pg_total_relation_size('"' || table_schema || '"."' || table_name || '"') DESC) t3
WHERE t3.table_full_name='public.tb_test_copy3'


SELECT t1.age,t1.lon,t1.lat from (SELECT generate_series(1,10000) as key,12 as age,138.000233 as lon,-23.763432 as lat) t1

INSERT INTO "public"."tb_test_copy1"("id", "age", "lon", "lat") VALUES (1, 12, '1', '1');



INSERT INTO "public"."tb_test_copy1"("age", "lon", "lat") SELECT t1.age,t1.lon,t1.lat from (SELECT generate_series(1,1000000) as key,99 as age,138.000233 as lon,-23.763432 as lat) t1

INSERT INTO "public"."tb_test_copy2"("age", "lon", "lat") SELECT t1.age,t1.lon,t1.lat from (SELECT generate_series(1,1000000) as key,99 as age,138.000233 as lon,-23.763432 as lat) t1

INSERT INTO "public"."tb_test_copy3"("age", "lon", "lat") SELECT t1.age,t1.lon,t1.lat from (SELECT generate_series(1,1000000) as key,99 as age,138.000233 as lon,-23.763432 as lat) t1


```

## 八、触发器自动写入变更日志

```
--1、trk_flt_path_catalog变化监测函数
CREATE OR REPLACE FUNCTION fun_trk_flt_path_catalog_watcher ( ) RETURNS TRIGGER AS $fun_trk_flt_path_catalog_watcher$ DECLARE
	d_flt_path_catalog_id int8;
  d_hex VARCHAR ( 8 );
  d_action_flag int2;
  d_content JSONB;
  d_option_time TIMESTAMP;
  d_tmp_is_insert bool DEFAULT FALSE;
  d_tmp_cotent TEXT;
BEGIN
	IF ( TG_OP = 'DELETE' ) THEN
			d_action_flag = 2;
		  d_flt_path_catalog_id = OLD.flt_path_catalog_id;
		  d_hex = OLD.hex;
			d_content=row_to_json(OLD)::jsonb;
		  d_tmp_is_insert = TRUE;
		ELSIF ( TG_OP = 'UPDATE' ) THEN
			d_action_flag = 3;
			d_flt_path_catalog_id = OLD.flt_path_catalog_id;
			d_hex = OLD.hex;
			--需要监测的变化字段
	
			IF ( OLD.flt_path_catalog_id != NEW.flt_path_catalog_id ) THEN
					d_tmp_cotent = concat_ws ( ',', d_tmp_cotent, json_build_object ( 'F','flt_path_catalog_id','O', OLD.flt_path_catalog_id, 'N', NEW.flt_path_catalog_id ) :: VARCHAR );
			END IF;
			IF ( OLD.hex != NEW.hex ) THEN
					d_tmp_cotent = concat_ws ( ',', d_tmp_cotent, json_build_object ( 'F','hex','O', OLD.hex, 'N', NEW.hex ) :: VARCHAR );
			END IF;
			IF ( OLD.reg != NEW.reg ) THEN
					d_tmp_cotent = concat_ws ( ',', d_tmp_cotent, json_build_object ( 'F','reg','O', OLD.reg, 'N', NEW.reg ) :: VARCHAR );
			END IF;
			--变化结果-jsonb
			IF ( LENGTH ( d_tmp_cotent ) > 0 ) THEN
				  d_tmp_is_insert = TRUE;
				  d_content = ( '[' || d_tmp_cotent || ']' ) :: jsonb;	
			END IF;
			ELSIF ( TG_OP = 'INSERT' ) THEN
				d_action_flag = 1;
				d_flt_path_catalog_id = NEW.flt_path_catalog_id;
				d_hex=NEW.hex;
					d_content=row_to_json(NEW)::jsonb;
				d_tmp_is_insert = TRUE;
				
			END IF;
-- 更新数据到汇总表
			IF  d_tmp_is_insert  THEN
					<< insert_update >>
					loop
				BEGIN
						INSERT INTO trk_flt_path_catalog_log ( flt_path_catalog_id, hex, action_flag, content, option_time)
					VALUES
						( d_flt_path_catalog_id, d_hex, d_action_flag, d_content, now( ));
					exit insert_update;
					EXCEPTION 
						WHEN unique_violation THEN-- 什么也不做
						
					END;
				
			END loop insert_update;

END IF;
RETURN NULL;

END;
$fun_trk_flt_path_catalog_watcher$ LANGUAGE plpgsql;

--创建触发器
DROP TRIGGER if EXISTS trigger_trk_flt_path_catalog_watcher ON trk_flt_path_catalog;

CREATE TRIGGER trigger_trk_flt_path_catalog_watcher AFTER INSERT OR UPDATE OR DELETE ON trk_flt_path_catalog FOR EACH ROW
EXECUTE PROCEDURE fun_trk_flt_path_catalog_watcher ();
	
	
	
	--测试SQL
	
	INSERT INTO "public"."trk_flt_path_catalog" ( flt_path_catalog_id, hex, reg ) VALUES
	( 1, '7800B7', 'A320' );
	
	DELETE FROM trk_flt_path_catalog WHERE flt_path_catalog_id=1;
  
	UPDATE trk_flt_path_catalog SET hex = '888886',reg='B323' WHERE flt_path_catalog_id = 1;
	
	SELECT t.* From trk_flt_path_catalog t;
	
	SELECT t.* from trk_flt_path_catalog_log t;
	
```

## 九、postgresql 数据字典

CREATE EXTENSION hstore;

[(34条消息) PostgreSQL hstore数据类型_梦想画家的博客-CSDN博客_hstore类型](https://blog.csdn.net/neweastsun/article/details/92849375)

十、postgresql树结构查询

```
--树状查询
WITH RECURSIVE T (dept_id, dept_name, parent_dept_id, PATH, DEPTH)  AS (
    SELECT dept_id, dept_name, parent_dept_id, ARRAY[dept_id] AS PATH, 1 AS DEPTH
    FROM app_dept
    WHERE parent_dept_id=-1
 
    UNION ALL
 
    SELECT  D.dept_id, D.dept_name, D.parent_dept_id, T.PATH || D.dept_id, T.DEPTH + 1 AS DEPTH
    FROM app_dept D
    JOIN T ON D.parent_dept_id = T.dept_id
    )
    SELECT dept_id, dept_name, parent_dept_id, PATH, DEPTH FROM T
ORDER BY PATH;


--创建树结构视图
CREATE VIEW view_app_dept AS
WITH RECURSIVE cte AS (
	SELECT
		A.dept_id,
		A.parent_dept_id,
		A.dept_name,
		A.dept_level,
		A.remark,
		A.update_time,
		ARRAY[A.dept_id] AS id_full_path, 
		CAST (A.dept_name AS Text) AS name_full_path,
		1 AS depth
	FROM
		app_dept A
	WHERE
		A.parent_dept_id=-1
	UNION ALL
		SELECT
			K.dept_id,
			K.parent_dept_id,
			K.dept_name,
			K.dept_level,
			K.remark,
		  K.update_time,
			C.id_full_path || K.dept_id,
			CAST (
				C.name_full_path || '->' || K.dept_name AS Text
			) AS name_full_path,
			C.depth + 1 AS depth
		FROM
			app_dept K
		INNER JOIN cte C ON C.dept_id = K .parent_dept_id
) 
SELECT	*  FROM  cte;






SELECT t.* from app_dept t
```



**ST_ShiftLongitude**

```
select t.id,ST_Intersects(ST_GeomFromText('SRID=4326;POINT(180 0)'),t.geom) from aaa t


select t.id,ST_Intersects(ST_GeomFromText(ST_ShiftLongitude('SRID=4326;POINT(180 0)'::geometry)),t.geom) from bbb t


SELECT ST_AsText(ST_ShiftLongitude('SRID=4326;POINT(270 0)'::geometry))

select t.id,ST_Intersects(ST_GeomFromText('SRID=4326;POINT(180 0)'),t.geom) from ccc t


select t.id,ST_Intersects(ST_GeomFromText('SRID=4326;POINT(180 0)'),t.geom) from bbb t



select t.id,ST_Intersects(ST_GeomFromText('SRID=4326;POINT(180 0)'),ST_ShiftLongitude(t.geom)) from bbb t



select t.id,ST_Intersects(ST_ShiftLongitude('SRID=4326;POINT(180 0)'::geometry),t.geom) from bbb t




SELECT ST_AsText(ST_ShiftLongitude('SRID=4326;POINT(270 0)'::geometry))





select t.id,ST_Intersects(ST_GeomFromText('SRID=4326;POINT(180 0)'),t.geom) from bbb t

SELECT t.*,ST_AsGeojson(ST_ShiftLongitude(t.geom)) from bbb t 



SELECT t.*,ST_AsGeojson(ST_ShiftLongitude(t.geom)) from aaa t 
SELECT t.*,ST_AsGeojson(t.geom) from aaa t 


SELECT t.*,ST_AsGeojson(t.geom) from bbb t 


select t.id,ST_Intersects(ST_GeomFromText('SRID=4326;POINT(180 0)'),ST_ShiftLongitude(t.geom)) from bbb t


select t.id,ST_Intersects(ST_GeomFromText('SRID=4326;POINT(180 0)'),ST_ShiftLongitude(t.geom)) from aaa t


select t.id,ST_Intersects(ST_GeomFromText('SRID=4326;POINT(180 0)'),ST_ShiftLongitude(t.geom)) from bbb t


select t.id,ST_Intersects(ST_GeomFromText('SRID=4326;POINT(180 0)'),t.geom) from ccc t


SELECT ST_ShiftLongitude('SRID=4326;POINT(270 0)'::geometry)



SELECT t.*,ST_AsGeojson(t.geom) from line t WHERE t.id=1
UNION
SELECT t2.*,ST_AsGeojson(ST_ShiftLongitude(t2.geom)) from line t2 WHERE t2.id=1



SELECT t.*,ST_AsGeojson(t.geom) from line t WHERE t.id=2
UNION
SELECT t2.*,ST_AsGeojson(ST_ShiftLongitude(t2.geom)) from line t2 WHERE t2.id=2



SELECT t.*,ST_AsGeojson(t.geom) from line t WHERE t.id=3
UNION
SELECT t2.*,ST_AsGeojson(ST_ShiftLongitude(t2.geom)) from line t2 WHERE t2.id=3


SELECT t2.*,ST_AsGeojson(ST_ShiftLongitude(t2.geom)) from bbb t2 WHERE t2.id=2
UNION
SELECT t2.*,ST_AsGeojson(t2.geom) from bbb t2 WHERE t2.id=2



select t.id,ST_Intersects(ST_ShiftLongitude('SRID=4326;POINT(270 0)'::geometry),ST_ShiftLongitude(t.geom)) from bbb t


SELECT ST_AsGeojson(ST_ShiftLongitude('SRID=4326;POINT(-180 0)'::geometry))



select t.id,ST_Intersects(ST_GeomFromText('SRID=4326;POINT(180 0)'),t.geom) from bbb t



SELECT ST_AsGeojson(ST_ShiftLongitude(ST_GeomFromGeoJSON('{"type":"MultiLineString","coordinates":[[[190,0],[200,0]]]}')))




select t.id,ST_Intersects(ST_ShiftLongitude('SRID=4326;POINT(195 10)'::geometry),t.geom) from bbb t



select t.id,ST_Intersects(ST_ShiftLongitude('SRID=4326;POINT(140 10)'::geometry),ST_ShiftLongitude(t.geom)) from bbb t





select t.id,ST_Intersects(ST_ShiftLongitude(ST_GeomFromText('SRID=4326;POINT(180 0)')),ST_ShiftLongitude(t.geom)) from bbb t


select t.id,ST_Intersects(ST_ShiftLongitude(ST_GeomFromGeoJSON('{ "type": "LineString", "coordinates": [  [ 123.040793768346759, 9.730444522056079 ], [ 137.433359600117768, -1.486022233349445 ], [ 154.641862225061402, 6.47845005364259 ], [ 154.641862225061402, 6.47845005364259 ]  ] }')),ST_ShiftLongitude(t.geom)) from bbb t


select t.id,ST_Intersects(ST_ShiftLongitude(ST_GeomFromGeoJSON('{ "type": "MultiLineString", "coordinates": [ [ [ 161.055940476176744, 21.60538057615328 ], [ 167.626459660246098, 15.528637022045546 ], [ 171.068160185234774, 21.750757021376756 ], [ 171.068160185234774, 21.750757021376756 ] ] ] }')),ST_ShiftLongitude(t.geom)) from bbb t



select t.id,ST_Intersects(ST_ShiftLongitude(ST_GeomFromGeoJSON('{ "type": "MultiLineString", "coordinates": [ [ [ 104.111440880908802, -9.421922973491039 ], [ 159.804413012544444, -11.268610657217513 ], [ -141.373796196739193, -12.798593282251906 ], [ -117.438333454772192, -23.77024016518779 ] ] ] }')),ST_ShiftLongitude(t.geom)) from bbb t



select t.id,ST_Intersects(ST_ShiftLongitude(ST_GeomFromGeoJSON('{ "type": "MultiLineString", "coordinates": [ [ [ 112.481030793949458, 21.96854578492945 ], [ 119.833754642789017, 19.112268336819227 ], [ 124.918084963795081, 22.041068073531672 ], [ 128.046903622875732, 25.688387013070098 ], [ 128.046903622875732, 25.688387013070098 ] ] ] }')),ST_ShiftLongitude(t.geom)) from bbb t








select t.id,ST_Intersects(ST_GeomFromGeoJSON('{"type":"LineString","coordinates":[[123.04687499999999,7.013667927566642],[159.609375,21.289374355860424],[164.53125,-2.811371193331128],[124.45312499999999,-22.593726063929296],[160.3125,-17.978733095556155],[161.015625,-39.90973623453718]]}'),t.geom) from ccc t



select t.id,ST_AsGeojson(ST_Intersection(ST_GeomFromGeoJSON('{"type":"LineString","coordinates":[[123.04687499999999,7.013667927566642],[159.609375,21.289374355860424],[164.53125,-2.811371193331128],[124.45312499999999,-22.593726063929296],[160.3125,-17.978733095556155],[161.015625,-39.90973623453718]]}'),t.geom)) from ccc t

select t.id,ST_AsGeojson(ST_Intersection(ST_GeomFromGeoJSON('{"type":"LineString","coordinates":[[123.04687499999999,7.013667927566642],[159.609375,21.289374355860424],[164.53125,-2.811371193331128],[124.45312499999999,-22.593726063929296],[160.3125,-17.978733095556155],[161.015625,-39.90973623453718]]}'),ST_Boundary(t.geom)::geometry(MULTILINESTRING,4326))) from ccc t



select t.id,ST_AsGeojson(ST_Intersection(t.geom,ST_GeomFromGeoJSON('{"type":"LineString","coordinates":[[123.04687499999999,7.013667927566642],[159.609375,21.289374355860424],[164.53125,-2.811371193331128],[124.45312499999999,-22.593726063929296],[160.3125,-17.978733095556155],[161.015625,-39.90973623453718]]}'))) from ccc t

SELECT ST_AsGeojson(ST_Ring(t.geom)) from ccc t





SELECT ST_AsGeojson(ST_Boundary(geom)::geometry(MULTILINESTRING,4326)) from ccc


SELECT geom::geometry(MULTILINESTRING,4326) from ccc









SELECT ST_AsGeojson(ST_Boundary(geom)::geometry(MULTILINESTRING,4326)) from ccc


SELECT ST_AsGeojson(ST_DumpPoints(t.geom).geometry) from ccc t

SELECT ST_AsGeojson(ST_MakeLine(t1.geom)) from (SELECT (ST_DumpPoints(geom)).geom from ccc) as t1


SELECT ST_AsGeojson(ST_Intersection(ST_MakeLine(t1.geom),ST_GeomFromGeoJSON('{"type":"LineString","coordinates":[[123.04687499999999,7.013667927566642],[159.609375,21.289374355860424],[164.53125,-2.811371193331128],[124.45312499999999,-22.593726063929296],[160.3125,-17.978733095556155],[161.015625,-39.90973623453718]]}'))) from (SELECT (ST_DumpPoints(geom)).geom from ccc) as t1


SELECT ST_AsGeojson(ST_MakeLine(t1.geom)) from (SELECT (ST_DumpPoints(geom)).geom from ccc) as t1

SELECT ST_AsGeojson(ST_MakeLine(t1.geom)) from (SELECT (ST_DumpPoints(ST_GeometryN(geom, generate_series(1, ST_NumGeometries(geom))))).geom AS geom FROM ccc) as t1

SELECT ST_DumpPoints(ST_GeometryN(geom, generate_series(1, ST_NumGeometries(geom)))) AS geom FROM ccc

SELECT (ST_DumpPoints(geom)).geom from ccc
SELECT (ST_DumpPoints(ST_GeometryN(geom, generate_series(1, ST_NumGeometries(geom))))).geom AS geom FROM ccc











select t.id,ST_AsGeojson(ST_Intersection(t.geom,ST_GeomFromGeoJSON('{"type":"LineString","coordinates":[[123.04687499999999,7.013667927566642],[159.609375,21.289374355860424],[164.53125,-2.811371193331128],[124.45312499999999,-22.593726063929296],[160.3125,-17.978733095556155],[161.015625,-39.90973623453718]]}'))) from naip_fusion_geo_firuir_polygon t
WHERE ST_Intersects(t.geom,ST_GeomFromGeoJSON('{"type":"LineString","coordinates":[[123.04687499999999,7.013667927566642],[159.609375,21.289374355860424],[164.53125,-2.811371193331128],[124.45312499999999,-22.593726063929296],[160.3125,-17.978733095556155],[161.015625,-39.90973623453718]]}'))








```

## 十、PostGIS Raster扩展

CREATE EXTENSION postgis_raster;

```
PostGIS 是一款基于 PostgreSQL 数据库的地理空间数据管理工具，提供了很多强大的功能和扩展，以下是其主要的 Extension：

postgis：该 Extension 提供了 PostGIS 的主要功能，包括空间数据类型、空间索引和空间查询等。
postgis_topology：该 Extension 提供了拓扑关系支持，可以对空间数据进行拓扑关系的检测和维护。
postgis_tiger_geocoder：该 Extension 提供了美国 TIGER 地图数据的地理编码支持，可以将地址信息转换为空间数据。
postgis_sfcgal：该 Extension 提供了高级的三维空间计算功能，包括三维空间关系和三维空间操作等。
postgis_raster：该 Extension 提供了栅格数据的支持，可以对栅格数据进行空间查询和空间分析。
postgis_fuzzystrmatch：该 Extension 提供了模糊匹配和字符串比较的功能，可以对字符串进行模糊匹配和相似性比较。
postgis_sfcgal：该 Extension 提供了高级的三维空间计算功能，包括三维空间关系和三维空间操作等。
postgis_tiger_geocoder：该 Extension 提供了美国 TIGER 地图数据的地理编码支持，可以将地址信息转换为空间数据。
pgrouting提供了对路网的分析支持，包括双向Dijkstra最短路径等10多种功能。也就是通俗所说的路径规划。centos系统需要单独安装该扩展。

部分操作函数：
SELECT PostGIS_GDAL_Version();
SELECT ST_Count (t.rast) from rst_z_57_5 t;
SELECT t.rid,t.rast,ST_PixelAsPolygons(t.rast) from rst_z_57_5 t WHERE st_intersects(t.rast, 'SRID=4326;POINT(102.7 37.2)', 4)=TRUE;
SELECT st_numbands(t.rast),ST_Value(t.rast, 1, 1, 1) As b1pval  from rst_z_57_5 t 
其他raster相关空间函数：https://www.osgeo.cn/postgis-manual/RT_reference.html

```

## 十一、Timescaledb时序数据库扩展插件

部署服务vip 172.30.5.86:5000,172.30.5.86:5001

后端真是地址为 172.30.5.68、172.30.5.69、172.30.5.70

PG版本15.0.1

![img](Postgresql笔记记录.assets/clip_image002.jpg)

 

 

Navicat 查询15版本会出现该问题

![img](Postgresql笔记记录.assets/clip_image003.png)

用pgadmin没有问题

## 十二、pg自带分区表示例

```
https://cloud.tencent.com/developer/article/2123226
https://blog.csdn.net/zhangyupeng0528/article/details/119423234
https_cloud.tencent.com/?url=https%3A%2F%2Fcloud.tencent.com%2Fdeveloper%2Farticle%2F1942828%3Fshare_token%3Dd816641a-fab8-4084-a1a2-ae4081821f2d


https://cloud.tencent.com/developer/article/2123226

*利用Pg自带分区+pg_cron自动简历分区  只能在
pg_cron 调度程序是在名为 postgres 的默认 PostgreSQL 数据库中设置的。这些 pg_cron 对象是在此 postgres 数据库中创建的，所有调度操作都在此数据库中运行。

使用数据库内置调度器
以pg_cron为例，每天下午 14 点创建下一天的分区表；
示例：
CREATE TABLE tab
(
    id   bigint GENERATED ALWAYS AS IDENTITY,
    ts   timestamp NOT NULL,
    data text
) PARTITION BY LIST ((ts::date));

CREATE TABLE tab_def PARTITION OF tab DEFAULT;


CREATE OR REPLACE FUNCTION create_tab_part() RETURNS integer
    LANGUAGE plpgsql AS
$$
DECLARE
    dateStr varchar;
BEGIN
    SELECT to_char(DATE 'tomorrow', 'YYYYMMDD') INTO dateStr;
    EXECUTE
        format('CREATE TABLE tab_%s (LIKE tab INCLUDING INDEXES)', dateStr);
    EXECUTE
        format('ALTER TABLE tab ATTACH PARTITION tab_%s FOR VALUES IN (%L)', dateStr, dateStr);
    RETURN 1;
END;
$$;

CREATE EXTENSION pg_cron;

SELECT cron.schedule('0 14 * * *', $$SELECT create_tab_part();$$);

SELECT cron.schedule_in_database(
'dsp_db_wtr_2_job1',
'* * * * *',
$$insert into test values((random()*300)::int,'test');$$,
'dsp_db_wtr_2',
'admin'//有权限的用户名
);

select * from cron.job_run_details

//创建分区表 sql:create table if not exists "public"."hx_wtr_gfs_01" partition of "public"."hx_wtr_gfs" for values in ('01');

--create sequence "hx_wtr_gfs_id_seq" increment by 1 minvalue 1 no maxvalue start with 1; 

CREATE TABLE "public"."hx_wtr_gfs" (
  "id" int8 NOT NULL DEFAULT nextval('hx_wtr_gfs_id_seq'::regclass),
  "base_time_seq" varchar(10),
  "pre_time_seq" varchar(3),
  "x" int4,
  "y" int4,
  "value" float4[][]
) partition by list("pre_time_seq")
;


CREATE INDEX "idx_hx_wtr_gfs" ON "public"."hx_wtr_gfs" USING btree (
  "base_time_seq" COLLATE "pg_catalog"."default" "pg_catalog"."text_ops" ASC NULLS LAST,
  "x" "pg_catalog"."int4_ops" ASC NULLS LAST,
  "y" "pg_catalog"."int4_ops" ASC NULLS LAST
);


ALTER TABLE "public"."hx_wtr_gfs" 
  OWNER TO "admin";

COMMENT ON COLUMN "public"."hx_wtr_gfs"."id" IS '自增主键';

COMMENT ON COLUMN "public"."hx_wtr_gfs"."base_time_seq" IS '时间序列(基数)';

COMMENT ON COLUMN "public"."hx_wtr_gfs"."pre_time_seq" IS '预测时间序列';

COMMENT ON COLUMN "public"."hx_wtr_gfs"."x" IS 'x轴编码';

COMMENT ON COLUMN "public"."hx_wtr_gfs"."y" IS 'y轴编码';

COMMENT ON COLUMN "public"."hx_wtr_gfs"."value" IS '气象预报值';



create table "public"."hx_wtr_gfs_001" partition of "public"."hx_wtr_gfs" for values in ('001');
create table "public"."hx_wtr_gfs_002" partition of "public"."hx_wtr_gfs" for values in ('002');
create table "public"."hx_wtr_gfs_003" partition of "public"."hx_wtr_gfs" for values in ('003');
create table "public"."hx_wtr_gfs_004" partition of "public"."hx_wtr_gfs" for values in ('004');
create table "public"."hx_wtr_gfs_005" partition of "public"."hx_wtr_gfs" for values in ('005');
create table "public"."hx_wtr_gfs_006" partition of "public"."hx_wtr_gfs" for values in ('006');
create table "public"."hx_wtr_gfs_007" partition of "public"."hx_wtr_gfs" for values in ('007');
create table "public"."hx_wtr_gfs_008" partition of "public"."hx_wtr_gfs" for values in ('008');
create table "public"."hx_wtr_gfs_009" partition of "public"."hx_wtr_gfs" for values in ('009');
create table "public"."hx_wtr_gfs_010" partition of "public"."hx_wtr_gfs" for values in ('010');
create table "public"."hx_wtr_gfs_011" partition of "public"."hx_wtr_gfs" for values in ('011');
create table "public"."hx_wtr_gfs_012" partition of "public"."hx_wtr_gfs" for values in ('012');
create table "public"."hx_wtr_gfs_013" partition of "public"."hx_wtr_gfs" for values in ('013');
create table "public"."hx_wtr_gfs_014" partition of "public"."hx_wtr_gfs" for values in ('014');
create table "public"."hx_wtr_gfs_015" partition of "public"."hx_wtr_gfs" for values in ('015');
create table "public"."hx_wtr_gfs_016" partition of "public"."hx_wtr_gfs" for values in ('016');
create table "public"."hx_wtr_gfs_017" partition of "public"."hx_wtr_gfs" for values in ('017');
create table "public"."hx_wtr_gfs_018" partition of "public"."hx_wtr_gfs" for values in ('018');
create table "public"."hx_wtr_gfs_019" partition of "public"."hx_wtr_gfs" for values in ('019');
create table "public"."hx_wtr_gfs_020" partition of "public"."hx_wtr_gfs" for values in ('020');
create table "public"."hx_wtr_gfs_021" partition of "public"."hx_wtr_gfs" for values in ('021');
create table "public"."hx_wtr_gfs_022" partition of "public"."hx_wtr_gfs" for values in ('022');
create table "public"."hx_wtr_gfs_023" partition of "public"."hx_wtr_gfs" for values in ('023');
create table "public"."hx_wtr_gfs_024" partition of "public"."hx_wtr_gfs" for values in ('024');
create table "public"."hx_wtr_gfs_025" partition of "public"."hx_wtr_gfs" for values in ('025');
create table "public"."hx_wtr_gfs_026" partition of "public"."hx_wtr_gfs" for values in ('026');
create table "public"."hx_wtr_gfs_027" partition of "public"."hx_wtr_gfs" for values in ('027');
create table "public"."hx_wtr_gfs_028" partition of "public"."hx_wtr_gfs" for values in ('028');
create table "public"."hx_wtr_gfs_029" partition of "public"."hx_wtr_gfs" for values in ('029');
create table "public"."hx_wtr_gfs_030" partition of "public"."hx_wtr_gfs" for values in ('030');
create table "public"."hx_wtr_gfs_031" partition of "public"."hx_wtr_gfs" for values in ('031');
create table "public"."hx_wtr_gfs_032" partition of "public"."hx_wtr_gfs" for values in ('032');
create table "public"."hx_wtr_gfs_033" partition of "public"."hx_wtr_gfs" for values in ('033');
create table "public"."hx_wtr_gfs_034" partition of "public"."hx_wtr_gfs" for values in ('034');
create table "public"."hx_wtr_gfs_035" partition of "public"."hx_wtr_gfs" for values in ('035');
create table "public"."hx_wtr_gfs_036" partition of "public"."hx_wtr_gfs" for values in ('036');
create table "public"."hx_wtr_gfs_037" partition of "public"."hx_wtr_gfs" for values in ('037');
create table "public"."hx_wtr_gfs_038" partition of "public"."hx_wtr_gfs" for values in ('038');
create table "public"."hx_wtr_gfs_039" partition of "public"."hx_wtr_gfs" for values in ('039');
create table "public"."hx_wtr_gfs_040" partition of "public"."hx_wtr_gfs" for values in ('040');
create table "public"."hx_wtr_gfs_041" partition of "public"."hx_wtr_gfs" for values in ('041');
create table "public"."hx_wtr_gfs_042" partition of "public"."hx_wtr_gfs" for values in ('042');
create table "public"."hx_wtr_gfs_043" partition of "public"."hx_wtr_gfs" for values in ('043');
create table "public"."hx_wtr_gfs_044" partition of "public"."hx_wtr_gfs" for values in ('044');
create table "public"."hx_wtr_gfs_045" partition of "public"."hx_wtr_gfs" for values in ('045');
create table "public"."hx_wtr_gfs_046" partition of "public"."hx_wtr_gfs" for values in ('046');
create table "public"."hx_wtr_gfs_047" partition of "public"."hx_wtr_gfs" for values in ('047');
create table "public"."hx_wtr_gfs_048" partition of "public"."hx_wtr_gfs" for values in ('048');


insert into "public"."hx_wtr_gfs"("base_time_seq","pre_time_seq","x","y","value") values('2023052400','001',1,1,array[[1.1,1.2,1.3],[2.1,2.2,2.3]]);

SELECT * from "public"."hx_wtr_gfs_001"
SELECT * from "public"."hx_wtr_gfs_002"

SELECT * from "public"."hx_wtr_gfs" where "pre_time_seq"='001'


insert into "public"."hx_wtr_gfs"("base_time_seq","pre_time_seq","x","y","value") SELECT t2."base_time_seq",t2."pre_time_seq",t2."x",t2."y",t2."value" from (select generate_series(1,100),'2023052400' as "base_time_seq",'001' as "pre_time_seq",floor(1 + random() * 1440) as "x",floor(1 + random() * 720) as "y",array[[1.1,1.2,1.3],[2.1,2.2,2.3],[2.1,2.2,2.3],[2.1,2.2,2.3],[2.1,2.2,2.3],[2.1,2.2,2.3],[2.1,2.2,2.3],[2.1,2.2,2.3],[2.1,2.2,2.3],[2.1,2.2,2.3],[2.1,2.2,2.3],[2.1,2.2,2.3],[2.1,2.2,2.3],[2.1,2.2,2.3],[2.1,2.2,2.3],[2.1,2.2,2.3],[2.1,2.2,2.3],[2.1,2.2,2.3],[2.1,2.2,2.3],[2.1,2.2,2.3],[2.1,2.2,2.3],[2.1,2.2,2.3],[2.1,2.2,2.3],[2.1,2.2,2.3],[2.1,2.2,2.3],[2.1,2.2,2.3],[2.1,2.2,2.3],[2.1,2.2,2.3]] as "value") t2 ;

insert into "public"."hx_wtr_gfs"("base_time_seq","pre_time_seq","x","y","value") SELECT t2."base_time_seq",t2."pre_time_seq",t2."x",t2."y",t2."value" from (select generate_series(1,1036800),'2023052400' as "base_time_seq",'001' as "pre_time_seq",floor(1 + random() * 1440) as "x",floor(1 + random() * 720) as "y",array[[1.44,1.55,1.66],
[2.44,2.55,2.66],
[3.44,2.55,2.66],
[4.44,2.55,2.66],
[5.44,2.55,2.66],
[6.44,2.55,2.66],
[7.44,2.55,2.66],
[8.44,2.55,2.66],
[9.44,2.55,2.66],
[10.44,2.55,2.66],
[11.44,2.55,2.66],
[12.44,2.55,2.66],
[13.44,2.55,2.66],
[14.44,2.55,2.66],
[15.44,2.55,2.66],
[16.44,2.55,2.66],
[17.44,2.55,2.66],
[18.44,2.55,2.66],
[19.44,2.55,2.66],
[20.44,2.55,2.66],
[21.44,2.55,2.66],
[22.44,2.55,2.66],
[23.44,2.55,2.66],
[24.44,2.55,2.66],
[25.44,2.55,2.66],
[26.44,2.55,2.66],
[27.44,2.55,2.66],
[28.44,2.55,2.66]] as "value") t2 ;

SELECT t.* from hx_wtr_gfs t WHERE t.base_time_seq='2023052400' AND t.pre_time_seq='001' AND t.x=1370 AND t.y=50


SELECT t.*,t.value[1][2] as "h1" from hx_wtr_gfs t WHERE t.base_time_seq='2023052400' AND t.pre_time_seq='001' AND t.value[1][2] >1.2

SELECT value[1:2][2],"value" from hx_wtr_gfs



SELECT count(*) from hx_wtr_gfs
SELECT t.* from hx_wtr_gfs t WHERE t.base_time_seq='2023052400' AND t.pre_time_seq='001' AND t.x<1370 and t.x>30 AND t.y=50
SELECT t.value from hx_wtr_gfs t WHERE t.base_time_seq='2023052400' AND t.pre_time_seq='001' AND t.x=1370 AND t.y=50

SELECT t.*,t.value[1][2] as "h1" from hx_wtr_gfs t WHERE t.base_time_seq='2023052400' AND t.pre_time_seq='001' AND t.value[1][2] >1.2

SELECT value[1:2][2],"value" from hx_wtr_gfs

SELECT t.x,t.y,t.value[2][2],value[3][1] from hx_wtr_gfs t WHERE t.base_time_seq='2023052400' AND t.pre_time_seq='001' AND t.x<1370 and t.x>30 AND t.y=50
UNION
SELECT t.x,t.y,t.value[2][2],value[3][1] from hx_wtr_gfs t WHERE t.base_time_seq='2023052400' AND t.pre_time_seq='002' AND t.x<1370 and t.x>30 AND t.y=50
UNION
SELECT t.x,t.y,t.value[2][2],value[3][1] from hx_wtr_gfs t WHERE t.base_time_seq='2023052400' AND t.pre_time_seq='002' AND t.x<1370 and t.x>30 AND t.y=50
--300点 10高度 6H  4
--6000





SELECT value[2][1] from hx_wtr_gfs WHERE base_time_seq='2023052400' AND pre_time_seq='001'




SELECT t.abc->'150' from (select '{ "150":[1.1, 1.2, 1.3], "200": [2.1,2.2,2.3]}'::jsonb as abc) t


SELECT t.abc#>'{150,0}' from (select '{ "150":[1.1, 1.2, 1.3], "200": [2.1,2.2,2.3]}'::jsonb as abc) t 

SELECT t.x,t.y,t.c_lon,t.c_lat,t."value"->'p150' as "tuv" from hx_wtr_gfs t WHERE t.base_time_seq='2023061312' and t.pre_hour_seq='02' AND t.x>10 and t.x<400 and t.y>10 and t.y<100









SELECT t.x,t.y,t.c_lon,t.c_lat,t."value"->'p150' as "tuv" from hx_wtr_gfs t WHERE t.base_time_seq='2023061300' and t.pre_hour_seq='44' AND t.x>10 and t.x<400 and t.y>10 and t.y<100

SELECT t.base_time_seq as "BaseTimeSeq",array_agg(t.pre_hour_seq) as "PreHourSeq",max(t.ftp_zip_deal_done_time) as "MaxDoneDate" from (SELECT t2.base_time_seq,t2.pre_hour_seq,t2.ftp_zip_deal_done_time from hx_wtr_gfs_tags t2 WHERE t2.status=2 ORDER BY t2.base_time_seq,t2.pre_hour_seq) t GROUP BY t.base_time_seq

SELECT t.* from (SELECT t2.base_time_seq,t2.pre_hour_seq,t2.ftp_zip_deal_done_time from hx_wtr_gfs_tags t2 WHERE t2.status=2 ORDER BY t2.base_time_seq,t2.pre_hour_seq) t



SELECT t.x,t.y,t.c_lon,t.c_lat,t."value"->'p150' as "tuv" from hx_wtr_gfs t WHERE t.base_time_seq='2023061300' and t.pre_hour_seq='44' AND t.x>10 and t.x<400 and t.y>10 and t.y<100


SELECT t.x,t.y,t.c_lon,t.c_lat,array[150,200,250],array[t."value"->'p150',t."value"->'p200',t."value"->'p250'] as "tuv" from hx_wtr_gfs t WHERE t.base_time_seq='2023061318' and t.pre_hour_seq='03' AND t.x=10 and t.y=100
UNION
SELECT t.x,t.y,t.c_lon,t.c_lat,array[300,350],array[t."value"->'p300',t."value"->'p350'] as "tuv" from hx_wtr_gfs t WHERE t.base_time_seq='2023061318' and t.pre_hour_seq='03' AND t.x=10 and t.y=100




SELECT t.* from hx_wtr_gfs t LIMIT 1




SELECT t.x as "X",t.y as "Y",t.c_lon as "Lo",t.c_lat as "La",t.base_time_seq || t.pre_hour_seq as "T",array[300,350] as "P",array[t."value"->'p300',t."value"->'p350'] as "TUV" from hx_wtr_gfs t WHERE t.base_time_seq='2023061318' and t.pre_hour_seq='03' AND t.x<400 and t.y>100




SELECT t.x as "X",t.y as "Y",t.c_lon as "Lo",t.c_lat as "La",t.base_time_seq || t.pre_hour_seq as "T",array[300,200,150] as "P",array[t."value"->'p300',t."value"->'p200',t."value"->'p150'] as "TUV" from hx_wtr_gfs t WHERE t.base_time_seq='2023061306' and (t.pre_hour_seq='01' OR t.pre_hour_seq='02' OR t.pre_hour_seq='03' OR t.pre_hour_seq='04' OR t.pre_hour_seq='05' OR t.pre_hour_seq='06' OR t.pre_hour_seq='07' OR t.pre_hour_seq='08') AND t.x=1 and t.y=1 UNION SELECT t.x as "X",t.y as "Y",t.c_lon as "Lo",t.c_lat as "La",t.base_time_seq || t.pre_hour_seq as "T",array[350,300,200,150] as "P",array[t."value"->'p350',t."value"->'p300',t."value"->'p200',t."value"->'p150'] as "TUV" from hx_wtr_gfs t WHERE t.base_time_seq='2023061306' and (t.pre_hour_seq='02' OR t.pre_hour_seq='03' OR t.pre_hour_seq='04' OR t.pre_hour_seq='05' OR t.pre_hour_seq='06' OR t.pre_hour_seq='07' OR t.pre_hour_seq='08' OR t.pre_hour_seq='09') AND t.x=2 and t.y=2 UNION SELECT t.x as "X",t.y as "Y",t.c_lon as "Lo",t.c_lat as "La",t.base_time_seq || t.pre_hour_seq as "T",array[350,300,200,150] as "P",array[t."value"->'p350',t."value"->'p300',t."value"->'p200',t."value"->'p150'] as "TUV" from hx_wtr_gfs t WHERE t.base_time_seq='2023061306' and (t.pre_hour_seq='12' OR t.pre_hour_seq='13' OR t.pre_hour_seq='14' OR t.pre_hour_seq='15' OR t.pre_hour_seq='16' OR t.pre_hour_seq='17' OR t.pre_hour_seq='18') AND t.x=6 and t.y=7





SELECT t.x as "X",t.y as "Y",t.c_lon as "Lo",t.c_lat as "La",t.base_time_seq || t.pre_hour_seq as "T",array[300,200,150] as "P",array[t."value"->'p300',t."value"->'p200',t."value"->'p150'] as "TUV" from hx_wtr_gfs t WHERE t.base_time_seq='2023061306' and (t.pre_hour_seq='01') AND (t.x>=1 and t.x<=10 and t.y>=1 and t.y<=10 and mod(t.x,2)=0 and mod(t.y,2)=0)



SELECT to_char(to_timestamp(t.base_time_seq, 'yyyyMMddHH24') +  (t.pre_hour_seq || ' hour')::interval,'yyyyMMddHH24') as "T" from hx_wtr_gfs t LIMIT 1

SELECT to_char(now(), 'yyyyMMddHH')

SELECT t.base_time_seq,t.pre_hour_seq from hx_wtr_gfs t LIMIT 1

SELECT 'abc'||(t.pre_hour_seq || ' hour') from hx_wtr_gfs t LIMIT 1


SELECT to_timestamp('2023061318', 'yyyyMMddHH24') 


SELECT t.x as "X",t.y as "Y",t.c_lon as "Lo",t.c_lat as "La",t.base_time_seq || t.pre_hour_seq as "T",array[300,200,150] as "P",array[t."value"->'p300',t."value"->'p200',t."value"->'p150'] as "TUV" from hx_wtr_gfs t WHERE t.base_time_seq='2023061306' and (t.pre_hour_seq='01') AND (t.x>=1 and t.x<=10 and t.y>=1 and t.y<=10 and mod(t.x,2)=0 and mod(t.y,2)=0)


SELECT to_char(to_timestamp(t.base_time_seq, 'yyyyMMddHH24') +  (t.pre_hour_seq || ' hour')::interval,'yyyyMMddHH24') as "T" from hx_wtr_gfs t LIMIT 1

SELECT to_char(now(), 'yyyyMMddHH')

SELECT t.base_time_seq,t.pre_hour_seq from hx_wtr_gfs t LIMIT 1

SELECT 'abc'||(t.pre_hour_seq || ' hour') from hx_wtr_gfs t LIMIT 1


SELECT to_timestamp('2023061318', 'yyyyMMddHH24') 




SELECT t.x as "X",t.y as "Y",t.c_lon as "Lo",t.c_lat as "La",to_char(to_timestamp(t.base_time_seq, 'yyyyMMddHH24') +  (t.pre_hour_seq || ' hour')::interval,'yyyyMMddHH24') as "T",array[300,200,150] as "P",array[t."value"->'p300',t."value"->'p200',t."value"->'p150'] as "V" from hx_wtr_gfs t WHERE t.base_time_seq='2023061306' and (t.pre_hour_seq='01' OR t.pre_hour_seq='02' OR t.pre_hour_seq='03' OR t.pre_hour_seq='04' OR t.pre_hour_seq='05' OR t.pre_hour_seq='06' OR t.pre_hour_seq='07' OR t.pre_hour_seq='08' OR t.pre_hour_seq='09' OR t.pre_hour_seq='10' OR t.pre_hour_seq='11' OR t.pre_hour_seq='12' OR t.pre_hour_seq='13' OR t.pre_hour_seq='14' OR t.pre_hour_seq='15' OR t.pre_hour_seq='16' OR t.pre_hour_seq='17' OR t.pre_hour_seq='18' OR t.pre_hour_seq='19' OR t.pre_hour_seq='20' OR t.pre_hour_seq='21' OR t.pre_hour_seq='22' OR t.pre_hour_seq='23') AND t.x=1 and t.y=1


SELECT array[(t."value"->'p300')::jsonb,t."value"->'p200'] from hx_wtr_gfs t LIMIT 1

SELECT array[t."value"->'p300',t."value"->'p200'] from hx_wtr_gfs t LIMIT 1

SELECT string_to_array(t."value"->'p300',',') from hx_wtr_gfs t LIMIT 1

SELECT string_to_array('[1],[2],[3]', ',')

SELECT string_to_array('[1],[2],[3]', ',')

SELECT ARRAY[t."value"#>'{p300,0}',t."value"#>'{p350,0}'] from hx_wtr_gfs t LIMIT 1





t.abc#>'{150,0}'
SELECT array[[1,1,1]] || t."value"->'p300' from hx_wtr_gfs t LIMIT 1

SELECT array_ndims(t."value"->'p300') from hx_wtr_gfs t LIMIT 1

SELECT jsonb_array_elements_text(t."value"->'p300') from hx_wtr_gfs t LIMIT 1

SELECT t."value"->'p300' from hx_wtr_gfs t LIMIT 1



SELECT ARRAY(SELECT jsonb_array_elements_text(t."value"->'p300'))::jsonb AS txt_arr FROM  hx_wtr_gfs t LIMIT 1



SELECT array[ARRAY(SELECT jsonb_array_elements_text(t."value"->'p300')),ARRAY(SELECT jsonb_array_elements_text(t."value"->'p350'))] FROM  hx_wtr_gfs t LIMIT 1


SELECT t.x as "X",t.y as "Y",t.c_lon as "Lo",t.c_lat as "La",to_char(to_timestamp(t.base_time_seq, 'yyyyMMddHH24') +  (t.pre_hour_seq || ' hour')::interval,'yyyyMMddHH24') as "T",array[300,200,150] as "P",array[ARRAY(SELECT jsonb_array_elements_text(t."value"->'p300')),ARRAY(SELECT jsonb_array_elements_text(t."value"->'p200')),ARRAY(SELECT jsonb_array_elements_text(t."value"->'p150'))] as "V" from hx_wtr_gfs t WHERE t.base_time_seq='2023061306' and (t.pre_hour_seq='01' OR t.pre_hour_seq='02' OR t.pre_hour_seq='03' OR t.pre_hour_seq='04' OR t.pre_hour_seq='05' OR t.pre_hour_seq='06' OR t.pre_hour_seq='07' OR t.pre_hour_seq='08' OR t.pre_hour_seq='09' OR t.pre_hour_seq='10' OR t.pre_hour_seq='11' OR t.pre_hour_seq='12' OR t.pre_hour_seq='13' OR t.pre_hour_seq='14' OR t.pre_hour_seq='15' OR t.pre_hour_seq='16' OR t.pre_hour_seq='17' OR t.pre_hour_seq='18' OR t.pre_hour_seq='19' OR t.pre_hour_seq='20' OR t.pre_hour_seq='21' OR t.pre_hour_seq='22' OR t.pre_hour_seq='23') AND t.x=1 and t.y=1


```





## 十二、pg自带分区表示例

1. PostgreSQL的数据变化捕获和实时通知——基于Listen-Notify和Server-Sent Events

[PostgreSQL的数据变化捕获和实时通知——基于Listen-Notify和Server-Sent Events | 开飞机的老张 (kaifeiji.cc)](https://kaifeiji.cc/post/change-data-capture-and-instant-notification-on-postgresql-via-listen-notify-and-server-sent-events/)

[PgNotification C# (CSharp) Code Examples - HotExamples](https://csharp.hotexamples.com/examples/-/PgNotification/-/php-pgnotification-class-examples.html)

```
CREATE TABLE measurement (
    city_id         int not null,
    logdate         date not null,
    peaktemp        int,
    unitsales       int
) PARTITION BY RANGE (logdate);

--建子分区measurement_y2018
CREATE TABLE measurement_y2018 PARTITION OF measurement FOR VALUES FROM ('2018-01-01') TO ('2019-01-01');
--建子分区measurement_y2019
CREATE TABLE measurement_y2019 PARTITION OF measurement FOR VALUES FROM ('2019-01-01') TO ('2020-01-01');

--在主表非分区列建索引
CREATE INDEX idx_measurement_peaktemp ON measurement(peaktemp);
CREATE INDEX idx_measurement_peaktemp_city_id ON measurement(city_id);
--在主表分区表分区列建索引
CREATE INDEX idx_measurement_peaktemp_1 ON measurement(logdate,peaktemp);
 --在子分区表分区列建索引
CREATE INDEX idx_measurement_peaktemp_y2018 ON measurement_y2018(logdate);
 --在子分区表非分区列建索引
CREATE INDEX idx_measurement_peaktemp_y2018_2 ON measurement_y2018(peaktemp);
  --在子分区表非分区列建复合索引
CREATE INDEX idx_measurement_peaktemp_y2018_1 ON measurement_y2018(logdate,peaktemp);
```



## 十三、强制删除数据库语句

```
UPDATE pg_database SET datistemplate=false where datname='template1';
UPDATE pg_database SET datallowconn = 'false' WHERE datname = 'adf_db_app';
SELECT pg_terminate_backend(pid) FROM pg_stat_activity WHERE datname = 'adf_db_app';
drop database adf_db_app;


关于授权的命令
-- 授予database的所有操作给一个角色
GRANT ALL PRIVILEGES ON DATABASE bosslist TO bosslist;
-- 授权当前database的指定schema的所有表的只读权限给一个角色
GRANT SELECT ON ALL TABLES IN SCHEMA public TO bosslist;
-- 将mytable.col1的SELECT、UPDATE赋予miriam_rw
GRANT SELECT (col1), UPDATE (col1) ON mytable TO miriam_rw;
```

## 十四、Dem数据分幅入库与查询

```
Dem分幅表结构：basic_dem_integrate

CREATE TABLE "public"."basic_dem_integrate" (
  "id" "pg_catalog"."int4" NOT NULL DEFAULT nextval('basic_dem_integrate_id_seq'::regclass),
  "airport_icao" "pg_catalog"."varchar" COLLATE "pg_catalog"."default",
  "integrated_dem_name" "pg_catalog"."varchar" COLLATE "pg_catalog"."default",
  "start_lon" "pg_catalog"."numeric",
  "start_lat" "pg_catalog"."numeric",
  "lon_pixel_size" "pg_catalog"."numeric",
  "lat_pixel_size" "pg_catalog"."numeric",
  "min_lon" "pg_catalog"."numeric",
  "max_lon" "pg_catalog"."numeric",
  "min_lat" "pg_catalog"."numeric",
  "max_lat" "pg_catalog"."numeric",
  "lon_size" "pg_catalog"."int4",
  "lat_size" "pg_catalog"."int4",
  "ele" "pg_catalog"."int4"[],
  PRIMARY KEY ("id")
)
;

ALTER TABLE "public"."basic_dem_integrate" 
  OWNER TO "acm_admin";

COMMENT ON COLUMN "public"."basic_dem_integrate"."id" IS 'ID';

COMMENT ON COLUMN "public"."basic_dem_integrate"."airport_icao" IS '机场四字码';

COMMENT ON COLUMN "public"."basic_dem_integrate"."integrated_dem_name" IS '净空一体化Dem文件名称';

COMMENT ON COLUMN "public"."basic_dem_integrate"."start_lon" IS '起点经度';

COMMENT ON COLUMN "public"."basic_dem_integrate"."start_lat" IS '起点纬度';

COMMENT ON COLUMN "public"."basic_dem_integrate"."lon_pixel_size" IS '像素大小经度';

COMMENT ON COLUMN "public"."basic_dem_integrate"."lat_pixel_size" IS '像素大小纬度';

COMMENT ON COLUMN "public"."basic_dem_integrate"."min_lon" IS '最小经度';

COMMENT ON COLUMN "public"."basic_dem_integrate"."max_lon" IS '最大经度';

COMMENT ON COLUMN "public"."basic_dem_integrate"."min_lat" IS '最小纬度';

COMMENT ON COLUMN "public"."basic_dem_integrate"."max_lat" IS '最大纬度';

COMMENT ON COLUMN "public"."basic_dem_integrate"."lon_size" IS '经度像素总个数';

COMMENT ON COLUMN "public"."basic_dem_integrate"."lat_size" IS '纬度像素总个数';

COMMENT ON COLUMN "public"."basic_dem_integrate"."ele" IS '高程数组';

CREATE INDEX idx_basic_dem_integrate ON basic_dem_integrate USING btree (min_lon, max_lon,min_lat,max_lat,airport_icao,ele);

C# Gdal入库代码


  private async void CropDem(string strAirportIcao)
        {
            // 初始化GDAL
            //Gdal.AllRegister();
            var kit = new GdalKit();
            var info = kit.GetGdalInfo();
            info.OgrDrivers.Sort();
            info.GdalDrivers.Sort();
            // 输入DEM文件路径
            string demFilePath = Path.Combine(AppDomain.CurrentDomain.BaseDirectory, $"IntegratedDem/{strAirportIcao}.tif");
            // 打开DEM数据集
            Dataset demDataset = Gdal.Open(demFilePath, Access.GA_ReadOnly);
            if (demDataset == null)
            {
                Console.WriteLine("无法打开DEM文件");
                return;
            }
            //按个数切割
            // 获取影像信息和投影
            int width = demDataset.RasterXSize;
            int height = demDataset.RasterYSize;
            string projection = demDataset.GetProjectionRef();
            // 设置瓦片大小-切割图幅的个数
            int tileSize = 2000;
            int idx = 0;
            //循环
            for (int x = 0; x < width; x += tileSize)
            {
                for (int y = 0; y < height; y += tileSize)
                {
                    idx++;
                    // 计算瓦片的行和列
                    int tileWidth = Math.Min(tileSize, width - x);
                    int tileHeight = Math.Min(tileSize, height - y);
                    // 创建瓦片新的栅格数据集
                    string tileFilename = $"{strAirportIcao.ToLower()}_integ_dem_{idx}";
                    string tileFilePath = string.Format("D:\\TempData\\{0}.tif", tileFilename);
                    Dataset tileDataset = Gdal.GetDriverByName("GTiff").Create(tileFilePath, tileWidth, tileHeight, 1, DataType.GDT_Int16, null);
                    tileDataset.SetProjection(projection);
                    //设置瓦片的栅数据集的像素坐标与其地理坐标之间的关系
                    double[] geotransform = new double[6];
                    demDataset.GetGeoTransform(geotransform);
                    double tileXOrigin = geotransform[0] + x * geotransform[1] + y * geotransform[2];
                    double tileYOrigin = geotransform[3] + x * geotransform[4] + y * geotransform[5];
                    geotransform[0] = tileXOrigin;
                    geotransform[3] = tileYOrigin;
                    tileDataset.SetGeoTransform(geotransform);
                    //从瓦片中复制数据集
                    Band band = demDataset.GetRasterBand(1);
                    int[] data = new int[tileWidth * tileHeight];
                    band.ReadRaster(x, y, tileWidth, tileHeight, data, tileWidth, tileHeight, 0, 0);
                    tileDataset.GetRasterBand(1).WriteRaster(0, 0, tileWidth, tileHeight, data, tileWidth, tileHeight, 0, 0);
                    //band.SetNoDataValue(-9999);
                    //tileDataset.GetRasterBand(1).SetNoDataValue(0);
                    //数据入库
                    double[] geotransformTile = new double[6];
                    tileDataset.GetGeoTransform(geotransformTile);
                    int widthTile = tileDataset.RasterXSize;
                    int heightTile = tileDataset.RasterYSize;
                    BasicDemIntegrate basicDemIntegrate = new BasicDemIntegrate()
                    {
                        AirportIcao = strAirportIcao,
                        IntegratedDemName = tileFilename,
                        StartLon = (decimal)geotransformTile[0],
                        StartLat = (decimal)geotransformTile[3],
                        LonPixelSize = (decimal)geotransformTile[1],
                        LatPixelSize = (decimal)geotransformTile[5],
                        MinLon = (decimal)geotransformTile[0],
                        MaxLon = (decimal)(geotransformTile[0] + widthTile * geotransformTile[1]),
                        MinLat = (decimal)(geotransformTile[3] + heightTile * geotransformTile[5]),
                        MaxLat = (decimal)geotransformTile[3],
                        LonSize = tileWidth,
                        LatSize = tileHeight,
                        Ele = data
                    };
                    int num = _freeSql.Insert(basicDemIntegrate).ExecuteAffrows();
                    if (num == 1)
                        Console.WriteLine($"{tileFilename}入库成功!");
                    // 关闭瓦片数据集
                    tileDataset.FlushCache();
                    tileDataset.Dispose();
                }
            }
            demDataset.Dispose();
            Console.WriteLine("完成！");
        }
        
Dem按经纬度查询SQL语句：
--103.8463 36.2278  -2025

SELECT t1."id",(((103.8463-t1.start_lon)/t1.lon_pixel_size)::INTEGER) as x,t1.lon_size,(((36.2278-t1.start_lat)/t1.lat_pixel_size)::INTEGER) as y,t1.lat_size from "basic_dem_integrate" t1 
WHERE 
t1.min_lon<=103.8463 and t1.max_lon>=103.8463 and t1.min_lat<=36.2278 and t1.max_lat>=36.2278 and t1.airport_icao='ZLLL'
--1458 731



SELECT t.ele[731*2000+1458] from basic_dem_integrate t WHERE t.id=29

--最终sql语句：
With Temp AS
(SELECT t1."id",(((103.8463-t1.start_lon)/t1.lon_pixel_size)::INTEGER) as x,t1.lon_size,(((36.2278-t1.start_lat)/t1.lat_pixel_size)::INTEGER) as y,t1.lat_size from "basic_dem_integrate" t1 
WHERE 
t1.min_lon<=103.8463 and t1.max_lon>=103.8463 and t1.min_lat<=36.2278 and t1.max_lat>=36.2278 and t1.airport_icao='ZLLL')
SELECT t.ele[Temp.y*Temp.lat_size+Temp.x] from basic_dem_integrate t 
LEFT JOIN Temp
ON t.id=Temp.id
WHERE t.id=Temp.id
```

## 十五、postgres_fdw使用示例

```
--admin用户执行授权：
GRANT USAGE ON FOREIGN DATA WRAPPER postgres_fdw TO adf_admin;
--ALTER FOREIGN DATA WRAPPER postgres_fdw SET VALIDATOR pg_catalog.pg_foreign_data_wrapper_validator;



CREATE EXTENSION postgres_fdw;


--GRANT USAGE ON FOREIGN SERVER foreign_server_adf_db_other TO adf_admin;

--然后使用CREATE SERVER创建一个外部服务器。在这个例子中 我们希望连接到一个位于主机192.83.123.89上 并且监听5432端口的PostgreSQL服务器。 在该远程服务器上要连接的数据库名为foreign_db：
CREATE SERVER foreign_server_adf_db_other
        FOREIGN DATA WRAPPER postgres_fdw
        OPTIONS (host '172.30.7.100', port '6060', dbname 'adf_db_other');
				
ALTER SERVER foreign_server_adf_db_other OWNER TO "adf_admin";
--也需要一个用CREATE USER MAPPING 定义的用户映射来标识将在远程服务器上使用的角色：
CREATE USER MAPPING FOR adf_admin
        SERVER foreign_server_adf_db_other
        OPTIONS (user 'adf_admin', password 'Cast@2023');

--现在就可以使用CREATE FOREIGN TABLE创建外部表了。 在这个例子中我们希望访问远程服务器上名为 some_schema.some_table的表。它的本地名称是 foreign_table：

CREATE FOREIGN TABLE foreign_table_tb_dept (
         "dept_id" "pg_catalog"."int8" NOT NULL,
  "dept_name" "pg_catalog"."varchar" COLLATE "pg_catalog"."default",
  "parent_dept_id" "pg_catalog"."int8"
)
        SERVER foreign_server_adf_db_other
        OPTIONS (schema_name 'public', table_name 'tb_dept');
				
				
ALTER FOREIGN TABLE foreign_table_tb_dept 
  OWNER TO "adf_admin";
	
--测试
SELECT t.* from foreign_table_tb_dept t
	
--删除SQL
DROP FOREIGN TABLE foreign_table_tb_dept;
DROP USER MAPPING FOR adf_admin SERVER foreign_server_adf_db_other;	
DROP SERVER foreign_server_adf_db_other;
```

## 十六：分区表有价值的文章

Postgresql分区表大量实例与分区建议（LIST / RANGE / HASH / 多级混合分区）

举报

>  pg14场景下测试 



### 1 分区建议总结

#### 建表建议

- 分区键离散，可以使用PARTITION BY LIST。按字符串匹配决定落入哪个分区。
- 分区键连续，比如整形、日期等，可以使用PARTITION BY RANGE。
- 分区键数据随机无规律或规律简单，可以使用PARTITION BY HASH，用hash函数打散数据。
- 分区键数据随机有规律，规律复杂，可以使用多级混合分区，使数据平均分散、减少耦合。
- 每个分区都是一个普通PG表：    
  - 可以指定表空间：例如按月份分区的场景，可以把历史非活跃数据通过表空间指定到慢速廉价存储上，新的热数据保存到快速存储上。
  - 可以指定并发度：热数据表定制并发度parallel_workers，查询自动使用并行查询。

#### 查询建议

后面慢慢补充。

- 不带分区键的查询 或 带分区键但涉及大部分分区表的查询 会使执行计划成倍增长，在分区表很多时会消耗大量内存。生成执行计划的时间也会变长（几千个分区时可能Planning time会超过Execution time）。
- 分区数量的增长应该在设计时就有预期，根据表大小评估，一般最好不要上千。
- 分区间如果没有数据依赖最好（比如按月份分区可以很方便的删除某一个分区），如果删除一个分区需要把部分数据调整到其他分区，新增一个分区需要从其他分区拿数据，这样效率会很差。

#### 官网建议

[5.11.6. Best Practices for Declarative Partitioning](https://cloud.tencent.com/developer/tools/blog-entry?target=https%3A%2F%2Fwww.postgresql.org%2Fdocs%2Fcurrent%2Fddl-partitioning.html)

The choice of how to partition a table should be made carefully, as the performance of query planning and execution can be negatively affected by poor design.

**One of the most critical design decisions will be the column or columns by which you partition your data.** Often the best choice will be to partition by the column or set of columns which most commonly appear in WHERE clauses of queries being executed on the partitioned table. WHERE clauses that are compatible with the partition bound constraints can be used to prune unneeded partitions. However, you may be forced into making other decisions by requirements for the PRIMARY KEY or a UNIQUE constraint. Removal of unwanted data is also a factor to consider when planning your partitioning strategy. An entire partition can be detached fairly quickly, so it may be beneficial to design the partition strategy in such a way that all data to be removed at once is located in a single partition.

Choosing the target number of partitions that the table should be divided into is also a critical decision to make. Not having enough partitions may mean that indexes remain too large and that data locality remains poor which could result in low cache hit ratios. However, dividing the table into too many partitions can also cause issues. Too many partitions can mean longer query planning times and higher memory consumption during both query planning and execution, as further described below. When choosing how to partition your table, it’s also important to consider what changes may occur in the future. For example, if you choose to have one partition per customer and you currently have a small number of large customers, consider the implications if in several years you instead find yourself with a large number of small customers. In this case, it may be better to choose to partition by HASH and choose a reasonable number of partitions rather than trying to partition by LIST and hoping that the number of customers does not increase beyond what it is practical to partition the data by.

Sub-partitioning can be useful to further divide partitions that are expected to become larger than other partitions. Another option is to use range partitioning with multiple columns in the partition key. Either of these can easily lead to excessive numbers of partitions, so restraint is advisable.

It is important to consider the overhead of partitioning during query planning and execution. The query planner is generally able to handle partition hierarchies with up to a few thousand partitions fairly well, provided that typical queries allow the query planner to prune all but a small number of partitions. Planning times become longer and memory consumption becomes higher when more partitions remain after the planner performs partition pruning. Another reason to be concerned about having a large number of partitions is that the server’s memory consumption may grow significantly over time, especially if many sessions touch large numbers of partitions. That’s because each partition requires its metadata to be loaded into the local memory of each session that touches it.

With data warehouse type workloads, it can make sense to use a larger number of partitions than with an OLTP type workload. Generally, in data warehouses, query planning time is less of a concern as the majority of processing time is spent during query execution. With either of these two types of workload, it is important to make the right decisions early, as re-partitioning large quantities of data can be painfully slow. Simulations of the intended workload are often beneficial for optimizing the partitioning strategy.

**Never just assume that more partitions are better than fewer partitions, nor vice-versa.** 永远不要假设更多的分区比更少的分区更好，反之亦然。

### 2 PARTITION BY LIST

分区键离散，可以使用PARTITION BY LIST。按字符串匹配决定落入哪个分区。

```javascript
drop table customers;
CREATE TABLE customers (id INTEGER, status TEXT, arr NUMERIC) PARTITION BY LIST(status);

CREATE TABLE cust_active PARTITION OF customers FOR VALUES IN ('ACTIVE');
CREATE TABLE cust_archived PARTITION OF customers FOR VALUES IN ('EXPIRED1', 'EXPIRED2');
CREATE TABLE cust_others PARTITION OF customers DEFAULT;

INSERT INTO customers VALUES (1,'ACTIVE',100), (2,'RECURRING',20), (3,'EXPIRED1',38), (4,'REACTIVATED',144);


-- 父表
postgres=# \d+ customers
                                  Partitioned table "public.customers"
 Column |  Type   | Collation | Nullable | Default | Storage  | Compression | Stats target | Description 
--------+---------+-----------+----------+---------+----------+-------------+--------------+-------------
 id     | integer |           |          |         | plain    |             |              | 
 status | text    |           |          |         | extended |             |              | 
 arr    | numeric |           |          |         | main     |             |              | 
Partition key: LIST (status)
Partitions: cust_active FOR VALUES IN ('ACTIVE'),
            cust_archived FOR VALUES IN ('EXPIRED'),
            cust_others DEFAULT

-- 子表
postgres=# \d+ cust_active
                                       Table "public.cust_active"
 Column |  Type   | Collation | Nullable | Default | Storage  | Compression | Stats target | Description 
--------+---------+-----------+----------+---------+----------+-------------+--------------+-------------
 id     | integer |           |          |         | plain    |             |              | 
 status | text    |           |          |         | extended |             |              | 
 arr    | numeric |           |          |         | main     |             |              | 
Partition of: customers FOR VALUES IN ('ACTIVE')
Partition constraint: ((status IS NOT NULL) AND (status = 'ACTIVE'::text))
Access method: heap
```

复制

带分区键查询：直接在指定表上查询。

```javascript
postgres=# explain select * from customers where status = 'EXPIRED1' and arr > 30;
                               QUERY PLAN                                
-------------------------------------------------------------------------
 Seq Scan on cust_archived customers  (cost=0.00..22.75 rows=1 width=68)
   Filter: ((arr > '30'::numeric) AND (status = 'EXPIRED1'::text))
```

复制

不带分区键查询：不知道数据在哪个子表，全部扫一遍。

```javascript
postgres=# explain select * from customers where arr > 30;
                                    QUERY PLAN                                     
-----------------------------------------------------------------------------------
 Append  (cost=0.00..66.12 rows=849 width=68)
   ->  Seq Scan on cust_active customers_1  (cost=0.00..20.62 rows=283 width=68)
         Filter: (arr > '30'::numeric)
   ->  Seq Scan on cust_archived customers_2  (cost=0.00..20.62 rows=283 width=68)
         Filter: (arr > '30'::numeric)
   ->  Seq Scan on cust_others customers_3  (cost=0.00..20.62 rows=283 width=68)
         Filter: (arr > '30'::numeric)
```

复制

### 2 PARTITION BY RANGE

分区键值连续，可以考虑使用PARTITION BY RANGE。

- 整形分区键可以引用关键字：最小值MINVALUE、最大值MAXVALUE。
- 其他类型：无

时间类型CASE：

```javascript
CREATE TABLE measurement (
    city_id         int not null,
    logdate         date not null,
    peaktemp        int,
    unitsales       int
) PARTITION BY RANGE (logdate);

CREATE TABLE measurement_y2006m02 PARTITION OF measurement FOR VALUES FROM ('2006-02-01') TO ('2006-03-01');

CREATE TABLE measurement_y2006m03 PARTITION OF measurement FOR VALUES FROM ('2006-03-01') TO ('2006-04-01');

...
CREATE TABLE measurement_y2007m11 PARTITION OF measurement FOR VALUES FROM ('2007-11-01') TO ('2007-12-01');

CREATE TABLE measurement_y2007m12 PARTITION OF measurement FOR VALUES FROM ('2007-12-01') TO ('2008-01-01') TABLESPACE fasttablespace;

CREATE TABLE measurement_y2008m01 PARTITION OF measurement FOR VALUES FROM ('2008-01-01') TO ('2008-02-01') WITH (parallel_workers = 4)
TABLESPACE fasttablespace;
```

复制

整形CASE：

```javascript
drop table customers;
CREATE TABLE customers (id INTEGER, status TEXT, arr NUMERIC) PARTITION BY RANGE(arr);
CREATE TABLE cust_arr_small PARTITION OF customers FOR VALUES FROM (MINVALUE) TO (25);
CREATE TABLE cust_arr_medium PARTITION OF customers FOR VALUES FROM (25) TO (75);
CREATE TABLE cust_arr_large PARTITION OF customers FOR VALUES FROM (75) TO (MAXVALUE);

INSERT INTO customers VALUES (1,'ACTIVE',100), (2,'RECURRING',20), (3,'EXPIRED',38), (4,'REACTIVATED',144);

-- 父表
postgres=# \d+ customers
                                  Partitioned table "public.customers"
 Column |  Type   | Collation | Nullable | Default | Storage  | Compression | Stats target | Description 
--------+---------+-----------+----------+---------+----------+-------------+--------------+-------------
 id     | integer |           |          |         | plain    |             |              | 
 status | text    |           |          |         | extended |             |              | 
 arr    | numeric |           |          |         | main     |             |              | 
Partition key: RANGE (arr)
Partitions: cust_arr_large FOR VALUES FROM ('75') TO (MAXVALUE),
            cust_arr_medium FOR VALUES FROM ('25') TO ('75'),
            cust_arr_small FOR VALUES FROM (MINVALUE) TO ('25')

-- 子表
postgres=# \d+ cust_arr_small
                                      Table "public.cust_arr_small"
 Column |  Type   | Collation | Nullable | Default | Storage  | Compression | Stats target | Description 
--------+---------+-----------+----------+---------+----------+-------------+--------------+-------------
 id     | integer |           |          |         | plain    |             |              | 
 status | text    |           |          |         | extended |             |              | 
 arr    | numeric |           |          |         | main     |             |              | 
Partition of: customers FOR VALUES FROM (MINVALUE) TO ('25')
Partition constraint: ((arr IS NOT NULL) AND (arr < '25'::numeric))
Access method: heap
```

复制

带分区键查询：

```javascript
postgres=# SELECT tableoid::regclass,* FROM customers;
    tableoid     | id |   status    | arr 
-----------------+----+-------------+-----
 cust_arr_small  |  2 | RECURRING   |  20
 cust_arr_medium |  3 | EXPIRED     |  38
 cust_arr_large  |  1 | ACTIVE      | 100
 cust_arr_large  |  4 | REACTIVATED | 144
 
postgres=# explain select * from customers where status = 'EXPIRED1' and arr > 130;
                                QUERY PLAN                                
--------------------------------------------------------------------------
 Seq Scan on cust_arr_large customers  (cost=0.00..22.75 rows=1 width=68)
   Filter: ((arr > '130'::numeric) AND (status = 'EXPIRED1'::text))
```

复制

不带分区键查询：

```javascript
postgres=# explain select * from customers where status = 'EXPIRED1';
                                    QUERY PLAN                                     
-----------------------------------------------------------------------------------
 Append  (cost=0.00..61.94 rows=12 width=68)
   ->  Seq Scan on cust_arr_small customers_1  (cost=0.00..20.62 rows=4 width=68)
         Filter: (status = 'EXPIRED1'::text)
   ->  Seq Scan on cust_arr_medium customers_2  (cost=0.00..20.62 rows=4 width=68)
         Filter: (status = 'EXPIRED1'::text)
   ->  Seq Scan on cust_arr_large customers_3  (cost=0.00..20.62 rows=4 width=68)
         Filter: (status = 'EXPIRED1'::text)
```

复制

### 3 PARTITION BY HASH

分区键值本身没有规律进行平均差分，可以用Hash取模离散。

```javascript
drop table customers;
CREATE TABLE customers (id INTEGER, status TEXT, arr NUMERIC) PARTITION BY HASH(id);
CREATE TABLE cust_part1 PARTITION OF customers FOR VALUES WITH (modulus 3, remainder 0);
CREATE TABLE cust_part2 PARTITION OF customers FOR VALUES WITH (modulus 3, remainder 1);
CREATE TABLE cust_part3 PARTITION OF customers FOR VALUES WITH (modulus 3, remainder 2);

INSERT INTO customers VALUES (1,'ACTIVE',100), (2,'RECURRING',20), (3,'EXPIRED',38), (4,'REACTIVATED',144);

-- 父表
postgres=# \d+ customers
                                  Partitioned table "public.customers"
 Column |  Type   | Collation | Nullable | Default | Storage  | Compression | Stats target | Description 
--------+---------+-----------+----------+---------+----------+-------------+--------------+-------------
 id     | integer |           |          |         | plain    |             |              | 
 status | text    |           |          |         | extended |             |              | 
 arr    | numeric |           |          |         | main     |             |              | 
Partition key: HASH (id)
Partitions: cust_part1 FOR VALUES WITH (modulus 3, remainder 0),
            cust_part2 FOR VALUES WITH (modulus 3, remainder 1),
            cust_part3 FOR VALUES WITH (modulus 3, remainder 2)

-- 子表
postgres=# \d+ cust_part1
                                        Table "public.cust_part1"
 Column |  Type   | Collation | Nullable | Default | Storage  | Compression | Stats target | Description 
--------+---------+-----------+----------+---------+----------+-------------+--------------+-------------
 id     | integer |           |          |         | plain    |             |              | 
 status | text    |           |          |         | extended |             |              | 
 arr    | numeric |           |          |         | main     |             |              | 
Partition of: customers FOR VALUES WITH (modulus 3, remainder 0)
Partition constraint: satisfies_hash_partition('24667'::oid, 3, 0, id)
Access method: heap
```

复制

### 4 多级混合分区

```javascript
drop table customers;
CREATE TABLE customers (id INTEGER, status TEXT, arr NUMERIC) PARTITION BY LIST(status);
-- 子表1
CREATE TABLE cust_active PARTITION OF customers FOR VALUES IN ('ACTIVE','RECURRING','REACTIVATED') PARTITION BY RANGE(arr);

CREATE TABLE cust_arr_small PARTITION OF cust_active FOR VALUES FROM (MINVALUE) TO (101) PARTITION BY HASH(id);
CREATE TABLE cust_part11 PARTITION OF cust_arr_small FOR VALUES WITH (modulus 2, remainder 0);
CREATE TABLE cust_part12 PARTITION OF cust_arr_small FOR VALUES WITH (modulus 2, remainder 1);

-- 子表2
CREATE TABLE cust_other PARTITION OF customers DEFAULT PARTITION BY RANGE(arr);

CREATE TABLE cust_arr_large PARTITION OF cust_other FOR VALUES FROM (101) TO (MAXVALUE) PARTITION BY HASH(id);
CREATE TABLE cust_part21 PARTITION OF cust_arr_large FOR VALUES WITH (modulus 2, remainder 0);
CREATE TABLE cust_part22 PARTITION OF cust_arr_large FOR VALUES WITH (modulus 2, remainder 1);
```

复制

![在这里插入图片描述](/../images/650330c187de9673445156789042bfaa.png)

在这里插入图片描述

数据分布实例：

```javascript
INSERT INTO customers VALUES (1,'ACTIVE',100), (2,'RECURRING',20), (3,'REACTIVATED',38), (4,'EXPIRED',144), (5,'EXPIRED',150), (6,'EXPIRED',160), (7,'EXPIRED',161);

postgres=# SELECT tableoid::regclass,* FROM customers;
  tableoid   | id |   status    | arr 
-------------+----+-------------+-----
 cust_part11 |  1 | ACTIVE      | 100
 cust_part11 |  2 | RECURRING   |  20
 cust_part12 |  3 | REACTIVATED |  38
 cust_part22 |  4 | EXPIRED     | 144
 cust_part22 |  5 | EXPIRED     | 150
 cust_part22 |  6 | EXPIRED     | 160
 cust_part22 |  7 | EXPIRED     | 161
```

复制

### 5 调整分区

```javascript
drop table customers;
CREATE TABLE customers (id INTEGER, status TEXT, arr NUMERIC) PARTITION BY LIST(status);

CREATE TABLE cust_active PARTITION OF customers FOR VALUES IN ('ACTIVE');
CREATE TABLE cust_archived PARTITION OF customers FOR VALUES IN ('EXPIRED1', 'EXPIRED2');
CREATE TABLE cust_others PARTITION OF customers DEFAULT;


postgres=# \d+ customers
                                  Partitioned table "public.customers"
 Column |  Type   | Collation | Nullable | Default | Storage  | Compression | Stats target | Description 
--------+---------+-----------+----------+---------+----------+-------------+--------------+-------------
 id     | integer |           |          |         | plain    |             |              | 
 status | text    |           |          |         | extended |             |              | 
 arr    | numeric |           |          |         | main     |             |              | 
Partition key: LIST (status)
Partitions: cust_active FOR VALUES IN ('ACTIVE'),
            cust_archived FOR VALUES IN ('EXPIRED1', 'EXPIRED2'),
            cust_others DEFAULT
```

复制

#### 5.1 删除分区

```javascript
ALTER TABLE customers DETACH PARTITION cust_others;

postgres=# \d+ customers
                                  Partitioned table "public.customers"
 Column |  Type   | Collation | Nullable | Default | Storage  | Compression | Stats target | Description 
--------+---------+-----------+----------+---------+----------+-------------+--------------+-------------
 id     | integer |           |          |         | plain    |             |              | 
 status | text    |           |          |         | extended |             |              | 
 arr    | numeric |           |          |         | main     |             |              | 
Partition key: LIST (status)
Partitions: cust_active FOR VALUES IN ('ACTIVE'),
            cust_archived FOR VALUES IN ('EXPIRED1', 'EXPIRED2')
```

复制

#### 5.2 【父表】【分区键】建索引：子表自动创建索引

分区键上的索引只有父表需要，只用于父表找到子表，所以无需再子表上创建。

```javascript
drop table customers;
CREATE TABLE customers (id INTEGER, status TEXT, arr NUMERIC) PARTITION BY LIST(status);
CREATE TABLE cust_active PARTITION OF customers FOR VALUES IN ('ACTIVE');
CREATE TABLE cust_archived PARTITION OF customers FOR VALUES IN ('EXPIRED1', 'EXPIRED2');
CREATE TABLE cust_others PARTITION OF customers DEFAULT;

CREATE INDEX customers_status_idx ON customers (status);

postgres=# \d+ customers
                                  Partitioned table "public.customers"
 Column |  Type   | Collation | Nullable | Default | Storage  | Compression | Stats target | Description 
--------+---------+-----------+----------+---------+----------+-------------+--------------+-------------
 id     | integer |           |          |         | plain    |             |              | 
 status | text    |           |          |         | extended |             |              | 
 arr    | numeric |           |          |         | main     |             |              | 
Partition key: LIST (status)
Indexes:
    "customers_status_idx" btree (status)
Partitions: cust_active FOR VALUES IN ('ACTIVE'),
            cust_archived FOR VALUES IN ('EXPIRED1', 'EXPIRED2'),
            cust_others DEFAULT

postgres=# \d+ cust_archived
                                      Table "public.cust_archived"
 Column |  Type   | Collation | Nullable | Default | Storage  | Compression | Stats target | Description 
--------+---------+-----------+----------+---------+----------+-------------+--------------+-------------
 id     | integer |           |          |         | plain    |             |              | 
 status | text    |           |          |         | extended |             |              | 
 arr    | numeric |           |          |         | main     |             |              | 
Partition of: customers FOR VALUES IN ('EXPIRED1', 'EXPIRED2')
Partition constraint: ((status IS NOT NULL) AND (status = ANY (ARRAY['EXPIRED1'::text, 'EXPIRED2'::text])))
Indexes:
    "cust_archived_status_idx" btree (status)
Access method: heap
```

复制

#### 5.3【父表】【非分区键】建索引：子表自动创建索引

非分区键上的索引会传播的子表上，自动创建。

```javascript
drop table customers;
CREATE TABLE customers (id INTEGER, status TEXT, arr NUMERIC) PARTITION BY LIST(status);
CREATE TABLE cust_active PARTITION OF customers FOR VALUES IN ('ACTIVE');
CREATE TABLE cust_archived PARTITION OF customers FOR VALUES IN ('EXPIRED1', 'EXPIRED2');
CREATE TABLE cust_others PARTITION OF customers DEFAULT;

CREATE INDEX customers_arr_idx ON customers (arr);

postgres=# \d+ customers
                                  Partitioned table "public.customers"
 Column |  Type   | Collation | Nullable | Default | Storage  | Compression | Stats target | Description 
--------+---------+-----------+----------+---------+----------+-------------+--------------+-------------
 id     | integer |           |          |         | plain    |             |              | 
 status | text    |           |          |         | extended |             |              | 
 arr    | numeric |           |          |         | main     |             |              | 
Partition key: LIST (status)
Indexes:
    "customers_arr_idx" btree (arr)
Partitions: cust_active FOR VALUES IN ('ACTIVE'),
            cust_archived FOR VALUES IN ('EXPIRED1', 'EXPIRED2'),
            cust_others DEFAULT

postgres=# \d+ cust_archived
                                      Table "public.cust_archived"
 Column |  Type   | Collation | Nullable | Default | Storage  | Compression | Stats target | Description 
--------+---------+-----------+----------+---------+----------+-------------+--------------+-------------
 id     | integer |           |          |         | plain    |             |              | 
 status | text    |           |          |         | extended |             |              | 
 arr    | numeric |           |          |         | main     |             |              | 
Partition of: customers FOR VALUES IN ('EXPIRED1', 'EXPIRED2')
Partition constraint: ((status IS NOT NULL) AND (status = ANY (ARRAY['EXPIRED1'::text, 'EXPIRED2'::text])))
Indexes:
    "cust_archived_arr_idx" btree (arr)
Access method: heap
```

复制

#### 5.3【父表】建索引：不希望所有子表自动建索引

增加ONLY关键字，只给父表创建索引；在使用alter index给某些子表建索引：

```javascript
drop table customers;
CREATE TABLE customers (id INTEGER, status TEXT, arr NUMERIC) PARTITION BY LIST(status);
CREATE TABLE cust_active PARTITION OF customers FOR VALUES IN ('ACTIVE');
CREATE TABLE cust_archived PARTITION OF customers FOR VALUES IN ('EXPIRED1', 'EXPIRED2');
CREATE TABLE cust_others PARTITION OF customers DEFAULT;

CREATE INDEX customers_arr_idx ON ONLY customers (arr);

postgres=# \d+ customers
                                  Partitioned table "public.customers"
 Column |  Type   | Collation | Nullable | Default | Storage  | Compression | Stats target | Description 
--------+---------+-----------+----------+---------+----------+-------------+--------------+-------------
 id     | integer |           |          |         | plain    |             |              | 
 status | text    |           |          |         | extended |             |              | 
 arr    | numeric |           |          |         | main     |             |              | 
Partition key: LIST (status)
Indexes:
    "customers_arr_idx" btree (arr) INVALID
Partitions: cust_active FOR VALUES IN ('ACTIVE'),
            cust_archived FOR VALUES IN ('EXPIRED1', 'EXPIRED2'),
            cust_others DEFAULT
```

复制

子表无索引

```javascript
postgres=# \d+ cust_archived
                                      Table "public.cust_archived"
 Column |  Type   | Collation | Nullable | Default | Storage  | Compression | Stats target | Description 
--------+---------+-----------+----------+---------+----------+-------------+--------------+-------------
 id     | integer |           |          |         | plain    |             |              | 
 status | text    |           |          |         | extended |             |              | 
 arr    | numeric |           |          |         | main     |             |              | 
Partition of: customers FOR VALUES IN ('EXPIRED1', 'EXPIRED2')
Partition constraint: ((status IS NOT NULL) AND (status = ANY (ARRAY['EXPIRED1'::text, 'EXPIRED2'::text])))
Access method: heap
```

复制

子表手动创建索引

```javascript
CREATE INDEX cust_archived_arr_idx ON cust_archived (arr);
ALTER INDEX customers_arr_idx ATTACH PARTITION cust_archived_arr_idx;

postgres=# \d+ cust_archived
                                      Table "public.cust_archived"
 Column |  Type   | Collation | Nullable | Default | Storage  | Compression | Stats target | Description 
--------+---------+-----------+----------+---------+----------+-------------+--------------+-------------
 id     | integer |           |          |         | plain    |             |              | 
 status | text    |           |          |         | extended |             |              | 
 arr    | numeric |           |          |         | main     |             |              | 
Partition of: customers FOR VALUES IN ('EXPIRED1', 'EXPIRED2')
Partition constraint: ((status IS NOT NULL) AND (status = ANY (ARRAY['EXPIRED1'::text, 'EXPIRED2'::text])))
Indexes:
    "cust_archived_arr_idx" btree (arr)
Access method: heap
```

复制

#### 5.4【父表】先建索引后建子表，子表索引自动建吗：会

非分区键上的索引会传播的子表上，自动创建。

```javascript
drop table customers;
CREATE TABLE customers (id INTEGER, status TEXT, arr NUMERIC) PARTITION BY LIST(status);
CREATE TABLE cust_active PARTITION OF customers FOR VALUES IN ('ACTIVE');

CREATE INDEX customers_arr_idx ON customers (arr);


CREATE TABLE cust_archived PARTITION OF customers FOR VALUES IN ('EXPIRED1', 'EXPIRED2');
CREATE TABLE cust_others PARTITION OF customers DEFAULT;


postgres=# \d+ cust_active
                                       Table "public.cust_active"
 Column |  Type   | Collation | Nullable | Default | Storage  | Compression | Stats target | Description 
--------+---------+-----------+----------+---------+----------+-------------+--------------+-------------
 id     | integer |           |          |         | plain    |             |              | 
 status | text    |           |          |         | extended |             |              | 
 arr    | numeric |           |          |         | main     |             |              | 
Partition of: customers FOR VALUES IN ('ACTIVE')
Partition constraint: ((status IS NOT NULL) AND (status = 'ACTIVE'::text))
Indexes:
    "cust_active_arr_idx" btree (arr)
Access method: heap

postgres=# \d+ cust_archived
                                      Table "public.cust_archived"
 Column |  Type   | Collation | Nullable | Default | Storage  | Compression | Stats target | Description 
--------+---------+-----------+----------+---------+----------+-------------+--------------+-------------
 id     | integer |           |          |         | plain    |             |              | 
 status | text    |           |          |         | extended |             |              | 
 arr    | numeric |           |          |         | main     |             |              | 
Partition of: customers FOR VALUES IN ('EXPIRED1', 'EXPIRED2')
Partition constraint: ((status IS NOT NULL) AND (status = ANY (ARRAY['EXPIRED1'::text, 'EXPIRED2'::text])))
Indexes:
    "cust_archived_arr_idx" btree (arr)
Access method: heap

postgres=# \d+ cust_others
                                       Table "public.cust_others"
 Column |  Type   | Collation | Nullable | Default | Storage  | Compression | Stats target | Description 
--------+---------+-----------+----------+---------+----------+-------------+--------------+-------------
 id     | integer |           |          |         | plain    |             |              | 
 status | text    |           |          |         | extended |             |              | 
 arr    | numeric |           |          |         | main     |             |              | 
Partition of: customers DEFAULT
Partition constraint: (NOT ((status IS NOT NULL) AND (status = ANY (ARRAY['ACTIVE'::text, 'EXPIRED1'::text, 'EXPIRED2'::text]))))
Indexes:
    "cust_others_arr_idx" btree (arr)
Access method: heap
```

## 十七：分区表与TimeScaleDb的区别与比较

- 创建分区表sql

  ```
  --创建表
  CREATE SEQUENCE "public"."otr_item_track_partition_id_seq" 
  INCREMENT 1
  MINVALUE  1
  MAXVALUE 9223372036854775807
  START 1
  CACHE 1;
  
  SELECT setval('"public"."otr_item_track_partition_id_seq"', 1, false);
  
  ALTER SEQUENCE "public"."otr_item_track_partition_id_seq"
  OWNED BY "public"."otr_item_track_partition"."id";
  
  ALTER SEQUENCE "public"."otr_item_track_partition_id_seq" OWNER TO "adf_admin";
  
  CREATE TABLE "public"."otr_item_track_partition" (
    "id" "pg_catalog"."int8" NOT NULL DEFAULT nextval('otr_item_track_partition_id_seq'::regclass),
    "hx" "pg_catalog"."varchar" COLLATE "pg_catalog"."default",
    "lo" "pg_catalog"."float8",
    "la" "pg_catalog"."float8",
    "pt" geometry(POINT, 4326),
    "he" "pg_catalog"."int4",
    "te" "pg_catalog"."timestamp"
  ) PARTITION BY RANGE (te);
  
  ALTER TABLE "public"."otr_item_track_partition" 
    OWNER TO "adf_admin";
  
  COMMENT ON COLUMN "public"."otr_item_track_partition"."id" IS '自增ID';
  
  COMMENT ON COLUMN "public"."otr_item_track_partition"."hx" IS '地址码';
  
  COMMENT ON COLUMN "public"."otr_item_track_partition"."lo" IS '经度';
  
  COMMENT ON COLUMN "public"."otr_item_track_partition"."la" IS '纬度';
  
  COMMENT ON COLUMN "public"."otr_item_track_partition"."pt" IS 'Geometry';
  
  COMMENT ON COLUMN "public"."otr_item_track_partition"."he" IS '高度';
  
  COMMENT ON COLUMN "public"."otr_item_track_partition"."te" IS '时间';
  
  --创建hx、pt的索引 主表创建索引，子表继承
  CREATE INDEX idx_otr_item_track_partition_hx ON otr_item_track_partition (hx);
  CREATE INDEX idx_otr_item_track_partition_pt ON otr_item_track_partition USING GIST (pt);
  
  
  --建子分区
  CREATE TABLE otr_item_track_partition_1220 PARTITION OF otr_item_track_partition FOR VALUES FROM ('2023-12-20') TO ('2023-12-21');
  CREATE TABLE otr_item_track_partition_1221 PARTITION OF otr_item_track_partition FOR VALUES FROM ('2023-12-21') TO ('2023-12-22');
  CREATE TABLE otr_item_track_partition_1222 PARTITION OF otr_item_track_partition FOR VALUES FROM ('2023-12-22') TO ('2023-12-23');
  
  
  --插入随机数据
  INSERT INTO public.otr_item_track_partition (hx, lo, la, pt, he, te)
  SELECT
      UPPER(SUBSTRING(md5(random()::text), 1, 6)),
      random() * 100,
      random() * 100,
      ST_SetSRID(ST_MakePoint(random() * 360 - 180, random() * 180 - 90), 4326),
      floor(random() * 100),
  		'2023-12-20'::timestamp + (random() * ('2023-12-22'::timestamp - '2023-12-20'::timestamp))
  FROM generate_series(1, 100) AS i;
  --按时间创建分区表，已存在数据和表分区 
  
  ```

  

- 创建时序TimeScaleDb表的sql

  ```
  --添加扩展
  CREATE EXTENSION IF NOT EXISTS timescaledb CASCADE;
  --创建表
  CREATE SEQUENCE "public"."otr_item_track_timescale_id_seq" 
  INCREMENT 1
  MINVALUE  1
  MAXVALUE 9223372036854775807
  START 1
  CACHE 1;
  
  SELECT setval('"public"."otr_item_track_timescale_id_seq"', 1, false);
  
  ALTER SEQUENCE "public"."otr_item_track_timescale_id_seq"
  OWNED BY "public"."otr_item_track_timescale"."id";
  
  ALTER SEQUENCE "public"."otr_item_track_timescale_id_seq" OWNER TO "adf_admin";
  
  
  CREATE TABLE "public"."otr_item_track_timescale" (
    "id" SERIAL8,
    "hx" "pg_catalog"."varchar" COLLATE "pg_catalog"."default",
    "lo" "pg_catalog"."float8",
    "la" "pg_catalog"."float8",
    "pt" geometry(POINT, 4326),
    "he" "pg_catalog"."int4",
    "te" "pg_catalog"."timestamp" NOT NULL,
  	CONSTRAINT otr_item_track_timescale_pkey PRIMARY KEY(id,te)
  )
  ;
  
  ALTER TABLE "public"."otr_item_track_timescale" 
    OWNER TO "adf_admin";
  
  COMMENT ON COLUMN "public"."otr_item_track_timescale"."id" IS '自增ID';
  
  COMMENT ON COLUMN "public"."otr_item_track_timescale"."hx" IS '地址码';
  
  COMMENT ON COLUMN "public"."otr_item_track_timescale"."lo" IS '经度';
  
  COMMENT ON COLUMN "public"."otr_item_track_timescale"."la" IS '纬度';
  
  COMMENT ON COLUMN "public"."otr_item_track_timescale"."pt" IS 'Geometry';
  
  COMMENT ON COLUMN "public"."otr_item_track_timescale"."he" IS '高度';
  
  COMMENT ON COLUMN "public"."otr_item_track_timescale"."te" IS '时间';
  
  --创建索引
  CREATE INDEX idx_otr_item_track_timescale_hx ON otr_item_track_timescale (hx);
  CREATE INDEX idx_otr_item_track_timescale_pt ON otr_item_track_timescale USING GIST (pt);
  --创建超表-注意te必须为主键,不能重复
  SELECT create_hypertable('otr_item_track_timescale', 'te', chunk_time_interval => interval '1 day');
  
  CREATE UNIQUE INDEX idx_otr_item_track_timescale_hx_time ON otr_item_track_timescale(hx, te);
  
  --表空间赋值
  
  CREATE TABLESPACE tablespace1 location '/usr/local/pgsql/data1';
  SELECT attach_tablespace('tablespace1', 'hyper_int');
  SELECT * FROM show_tablespaces('otr_item_track_timescale');
  --插入示例数据
  INSERT INTO public.otr_item_track_timescale (hx, lo, la, pt, he, te)
  SELECT
      UPPER(SUBSTRING(md5(random()::text), 1, 6)),
      random() * 100,
      random() * 100,
      ST_SetSRID(ST_MakePoint(random() * 360 - 180, random() * 180 - 90), 4326),
      floor(random() * 100),
  		'2023-12-20'::timestamp + (random() * ('2023-12-22'::timestamp - '2023-12-20'::timestamp))
  FROM generate_series(1, 100) AS i;
  
  
  INSERT INTO "public"."otr_item_track_timescale" ("hx", "lo", "la", "pt", "he", "te") VALUES ('DF730C', '67.45588335746955', '4.262225556363464', ST_GeomFromText('POINT(-141.055104568401 -20.7978220169101)'), 92, '2023-12-21 21:05:36');
  INSERT INTO "public"."otr_item_track_timescale" ("hx", "lo", "la", "pt", "he", "te") VALUES ('DF730C', '67.45588335746955', '4.262225556363464', ST_GeomFromText('POINT(-141.055104568401 -20.7978220169101)'), 92, '2023-12-21 21:05:36');
  ```

  

- 性能对比

  ```
  --插入性能对比
  
  INSERT INTO public.otr_item_track_partition (hx, lo, la, pt, he, te)
  SELECT
      UPPER(SUBSTRING(md5(random()::text), 1, 6)),
      random() * 100,
      random() * 100,
      ST_SetSRID(ST_MakePoint(random() * 360 - 180, random() * 180 - 90), 4326),
      floor(random() * 100),
  		'2023-12-20'::timestamp + (random() * ('2023-12-22'::timestamp - '2023-12-20'::timestamp))
  FROM generate_series(1, 1000000) AS i;
  
  
  INSERT INTO public.otr_item_track_timescale (hx, lo, la, pt, he, te)
  SELECT
      UPPER(SUBSTRING(md5(random()::text), 1, 6)),
      random() * 100,
      random() * 100,
      ST_SetSRID(ST_MakePoint(random() * 360 - 180, random() * 180 - 90), 4326),
      floor(random() * 100),
  		'2023-12-20'::timestamp + (random() * ('2023-12-22'::timestamp - '2023-12-20'::timestamp))
  FROM generate_series(1, 1000000) AS i;
  
  
  
  --查询性能对比
  
  
  SELECT * FROM otr_item_track_partition WHERE hx like 'A%' and te>'2023-12-20 00:00:00' AND te<'2023-12-20 01:00:00';
  
  SELECT * FROM otr_item_track_timescale WHERE hx like 'A%' and te>'2023-12-20 00:00:00' AND te<'2023-12-20 01:00:00';
  
  --存储空间比较
  --查看表
  SELECT
    table_schema || '.' || table_name AS table_full_name,
    pg_size_pretty(total_size) AS total_size
  FROM (
    SELECT
      table_schema,
      table_name,
      pg_total_relation_size(table_schema || '.' || table_name) AS total_size
    FROM information_schema.tables ORDER BY total_size DESC
  ) AS table_sizes
  ;
  
  --清理表空间
  TRUNCATE otr_item_track ,otr_item_track_partition,otr_item_track_partition_1220,otr_item_track_partition_1221,otr_item_track_partition_1222 RESTART IDENTITY;
  truncate table otr_item_track_timescale,otr_item_track_partition,otr_item_track_partition_1220,otr_item_track_partition_1221,otr_item_track_partition_1222;
  
  VACUUM otr_item_track_partition;
  VACUUM otr_item_track_partition_1220;
  VACUUM otr_item_track_partition_1221;
  VACUUM otr_item_track_partition_1222;
  VACUUM otr_item_track_timescale;
  
  --200w数据存储比较
  
  --优势
    ----按小时桶化查询
  SELECT
    time_bucket('1 HOUR', te) AS bucket,
    AVG(he) AS average_value
  FROM otr_item_track_timescale
  WHERE te>'2023-12-20 00:00:00' AND te<'2023-12-20 05:00:00'
  GROUP BY bucket;
  
  
  
  
  --timescaledb压缩示例--使用timescaledb压缩比为70%
  ALTER TABLE public.otr_item_track_timescale SET (
    timescaledb.compress,
    timescaledb.compress_segmentby = 'hx'
  );
  --压缩策略
  SELECT add_compression_policy('otr_item_track_timescale', INTERVAL '1 days');
  SELECT * FROM timescaledb_information.jobs WHERE proc_name='policy_compression';
  
  
  SELECT show_chunks('otr_item_track_timescale', older_than => INTERVAL '1 days');
  
  --压缩块
  SELECT compress_chunk( '_timescaledb_internal._hyper_1_4_chunk');
  SELECT compress_chunk( '_timescaledb_internal._hyper_1_5_chunk');
  
  SELECT t.* FROM otr_item_track_timescale t WHERE t.hx='C1' AND t.te>'2023-12-20 00:00:00' AND t.te<'2023-12-20 05:00:00'
  --解压缩
  SELECT decompress_chunk('_timescaledb_internal._hyper_1_4_chunk');
  SELECT decompress_chunk('_timescaledb_internal._hyper_1_5_chunk');
  --查询压缩后空间状态
  
  SELECT * FROM chunk_compression_stats('otr_item_track_timescale');
  
  
  示例数据插入:
  
  INSERT INTO public.otr_item_track_partition (hx, lo, la, pt, he, te)
  SELECT
      'C' || CAST(FLOOR(RANDOM() * 100) + 1 AS TEXT),
      random() * 100,
      random() * 100,
      ST_SetSRID(ST_MakePoint(random() * 360 - 180, random() * 180 - 90), 4326),
      floor(random() * 100),
  		'2023-12-20'::timestamp + (random() * ('2023-12-22'::timestamp - '2023-12-20'::timestamp))
  FROM generate_series(1, 1000000) AS i;
  
  
  INSERT INTO public.otr_item_track_timescale (hx, lo, la, pt, he, te)
  SELECT
      'C' || CAST(FLOOR(RANDOM() * 100) + 1 AS TEXT),
      random() * 100,
      random() * 100,
      ST_SetSRID(ST_MakePoint(random() * 360 - 180, random() * 180 - 90), 4326),
      floor(random() * 100),
  		'2023-12-20'::timestamp + (random() * ('2023-12-22'::timestamp - '2023-12-20'::timestamp))
  FROM generate_series(1, 1000000) AS i;
  
  
  
  
  
  
  
  
  
  
  
  --其他可能有用的sql
  
  SELECT remove_compression_policy('cpu');
  ALTER TABLE <TABLE_NAME> SET (timescaledb.compress=false);
  
  	
  SELECT show_chunks('otr_item_track_timescale');
  	
  SELECT * FROM chunks_detailed_size('otr_item_track_timescale') ORDER BY chunk_name, node_name;
  	
  		
  SELECT * FROM show_tablespaces('otr_item_track_timescale');
  
  select * from pg_settings where name = 'timescaledb.disable_load'
  
  
  
  --解压缩
  
  SELECT j.job_id
      FROM timescaledb_information.jobs j
      WHERE j.proc_name = 'policy_compression'
          AND j.hypertable_name = 'otr_item_track_timescale';
  
  SELECT decompress_chunk('_timescaledb_internal.<chunk_name>');
  
  SELECT decompress_chunk(c, true) FROM show_chunks('table_name', older_than, newer_than) c;
  
  SELECT tableoid::regclass FROM otr_item_track_timescale
    WHERE te = '2023-12-20'
    GROUP BY tableoid;
  
  
  ```

  推荐文章：[timescaledb详细使用手册 | ALLBS](https://www.allbs.cn/posts/21383/index.html)

- 结果总监

  ```
  分区表和 TimescaleDB 超表都是用于处理大规模数据的方案，但它们有一些不同之处。在存储海量 GPS 数据的场景中，选择合适的方案取决于你的具体需求、查询模式和性能优化目标。以下是一些比较考虑的因素：
  
  1. 查询性能：
  
  分区表： 通过合理设计分区键，可以加速特定时间范围内的查询。但在一些查询中，可能需要跨越多个分区，性能可能会受到影响。
  TimescaleDB 超表： 针对时间序列数据进行了优化，支持自动数据分散和查询优化。在时间相关的查询上，可能会比手动分区表更为高效。
  2. 数据插入性能：
  
  分区表： 插入性能通常较好，因为数据分散到不同的分区，避免了单一表上的锁竞争。
  TimescaleDB 超表： 在处理时间序列数据的场景下，插入性能也是优化的重点，可以进行批量插入等优化。
  3. 数据维护：
  
  分区表： 需要手动管理分区的创建、删除、合并等操作，维护相对较复杂。
  TimescaleDB 超表： 自动处理数据分散和分区管理，减轻了维护工作的负担。
  4. 空间利用：
  
  分区表： 可以手动管理数据的存储位置，根据实际需求进行优化。
  TimescaleDB 超表： 通过数据分散和压缩等技术，可以有效减小存储空间占用。
  5. 索引和查询优化：
  
  分区表： 索引的选择和查询优化需要手动进行管理。
  TimescaleDB 超表： 内置了时间序列数据的索引和查询优化，减轻了用户的工作量。
  6. 数据类型支持：
  
  分区表： 支持任何 PostgreSQL 支持的数据类型。
  TimescaleDB 超表： 主要用于时间序列数据，对于其他类型的数据可能需要考虑。
  对于存储海量 GPS 数据，TimescaleDB 的超表通常是一个很好的选择，因为它专注于时间序列数据，提供了许多优化以及自动分散和分区管理的功能。然而，具体的选择取决于你的具体需求和对数据存储、查询性能和维护的优先级。最好的方式是在你的环境中进行性能测试，以了解哪种方案更适合你的用例。
  ```


## 十八、ADF公共建库SQL语句

```
adf数据库样例sql:
 --新建角色：
 CREATE ROLE "adf_admin" CREATEDB CREATEROLE LOGIN REPLICATION PASSWORD 'Cast@2023';
 --新建表空间
 CREATE TABLESPACE "tbs_adf" OWNER "adf_admin" LOCATION '/home/data/tblspc/tbs_adf';
  注：如果新建不成功，找李虹昊开表空间目录权限
  注：如果报被占用，则删除session 
  SELECT pg_terminate_backend(pid) FROM pg_stat_activity WHERE datname='template1' AND pid<>pg_backend_pid(); 

 --新建数据库：
CREATE DATABASE "adf_db_app"
WITH
  OWNER = "adf_admin"
  TABLESPACE = "tbs_adf"//
;

CREATE DATABASE "adf_db_other"
WITH
  OWNER = "adf_admin"
  TABLESPACE = "tbs_adf"//
;

或用navcat管理数据库扩展
CREATE EXTENSION postgis;
CREATE EXTENSION pg_partman;

赋予角色数据库所有权限：
GRANT ALL PRIVILEGES ON DATABASE adf_db_app TO adf_admin;
GRANT ALL PRIVILEGES ON DATABASE adf_db_other TO adf_admin;

更改表的owner：
alter table xxx owner to new_owner
  
SELECT
'alter table ' || nsp.nspname || '.' || cls.relname || ' owner to usr_zhudong;' || chr ( 13 )
FROM
pg_catalog.pg_class cls,
pg_catalog.pg_namespace nsp
WHERE
nsp.nspname IN ( 'public' )
AND cls.relnamespace = nsp.oid
AND cls.relkind = 'r'
ORDER BY
nsp.nspname,
cls.relname;

更改所有数据库的owner:
alter database imsdb owner to rcb
```

## 十九、Postgresql Rule的使用示例

```
--1、创建原表mytab
create table mytab(id int primary key,note text);
--创建记录表mytab_log
create table mytab_log(seq bigserial primary key,
oprtype char(1),
oprtime timestamp,
old_id int,
new_id int,
old_note text,
new_note text);
--2、创建规则
create rule rule_mytab_insert as on insert
to mytab
do also insert into mytab_log(oprtype,oprtime,new_id,new_note) values('i',now(),new.id,new.note);

create rule rule_mytab_update as on update
to mytab
do also insert into mytab_log(oprtype,oprtime,old_id,new_id,old_note,new_note) values('u',now(),old.id,new.id,old.note,new.note);
--（更新数据时实现记录旧数据和新数据，类型为”u“）
create rule rule_mytab_delete as on delete
to mytab
do also insert into mytab_log(oprtype,oprtime,old_id,old_note) values('d',now(),old.id,old.note);

insert into mytab values(1,'abc');
insert into mytab values(2,'bac');
insert into mytab values(3,'cab');

update mytab set note='abc2' WHERE id=1;

SELECT t.* from mytab_log t

```

![image-20240118104155384](Postgresql笔记记录.assets/image-20240118104155384.png)

## 二十、Postgresql的Listen-Notify机制

```plsql
CREATE FUNCTION public."NotifyOnDataChange"()
  RETURNS trigger
  LANGUAGE 'plpgsql'
AS $BODY$ 
DECLARE 
  data JSON;
  notification JSON;
BEGIN
  -- if we delete, then pass the old data
  -- if we insert or update, pass the new data
  IF (TG_OP = 'DELETE') THEN
    data = row_to_json(OLD);
  ELSE
    data = row_to_json(NEW);
  END IF;

  -- create json payload
  -- note that here can be done projection 
  notification = json_build_object(
            'table',TG_TABLE_NAME,
            'action', TG_OP, -- can have value of INSERT, UPDATE, DELETE
            'data', data);  
            
    -- note that channel name MUST be lowercase, otherwise pg_notify() won't work
    PERFORM pg_notify('datachange', notification::TEXT);
  RETURN NEW;
END
$BODY$;



CREATE TRIGGER "OnDataChange"
  AFTER INSERT OR DELETE OR UPDATE 
  ON public.orders
  FOR EACH ROW
  EXECUTE PROCEDURE public."NotifyOnDataChange"();
  
  
  
CREATE FUNCTION public."CreateOnDataChangeForAllTables"()
  RETURNS void
  LANGUAGE 'plpgsql'
AS $BODY$
DECLARE  
  createTriggerStatement TEXT;
BEGIN
  FOR createTriggerStatement IN SELECT
    'CREATE TRIGGER OnDataChange AFTER INSERT OR DELETE OR UPDATE ON '
    || tab_name
    || ' FOR EACH ROW EXECUTE PROCEDURE public."NotifyOnDataChange"();' AS trigger_creation_query
  FROM (
    SELECT
      quote_ident(table_schema) || '.' || quote_ident(table_name) as tab_name
    FROM
      information_schema.tables
    WHERE
      table_schema NOT IN ('pg_catalog', 'information_schema')
      AND table_schema NOT LIKE 'pg_toast%'
  ) as TableNames
  LOOP
    EXECUTE  createTriggerStatement; --actually create the trigger
  END LOOP;
END$BODY$;
```

```c#
class Program
{
  static async Task Main(string[] args)
  {
    const string connString = "<connection string>";

    await using var conn = new NpgsqlConnection(connString);
    await conn.OpenAsync();

    //e.Payload is string representation of JSON we constructed in NotifyOnDataChange() function
    conn.Notification += (o, e) => Console.WriteLine("Received notification: " + e.Payload);

    await using (var cmd = new NpgsqlCommand("LISTEN datachange;", conn))
      cmd.ExecuteNonQuery();

    while (true) 
      conn.Wait(); // wait for events
  }
}
```

## 二十一、Postgresql 计算列（Generated Columns）

```plsql
示例一：
-- Creating the employees table
CREATE TABLE employees (
    id SERIAL PRIMARY KEY,
    first_name VARCHAR(50),
    last_name VARCHAR(50),
    full_name VARCHAR(100) GENERATED ALWAYS AS (first_name || ' ' || last_name) STORED,
	temp_name VARCHAR(20) GENERATED ALWAYS AS (CASE WHEN first_name='John' THEN 'one' WHEN first_name='Jane' THEN 'two' ELSE 'other' END) STORED
);

-- Inserting some sample data
INSERT INTO employees (first_name, last_name) VALUES
    ('John', 'Doe'),
    ('Jane', 'Smith'),
		();

-- Querying the data
SELECT * FROM employees;




```

## 二十二、Duckdb

```
安装duckdb.cli
winget install DuckDB.cli

1、查询本地parquet文件，注意duckdb支持的类型
SELECT * FROM read_parquet('D:/temp/demo.parquet') WHERE X=1;
2、查询https协议的parquet文件
INSTALL httpfs;  
LOAD httpfs;  
SELECT * FROM read_parquet('https://<domain>/path/to/file.parquet');  
3、查询S3协议minio中的parquet文件

--1.安装并加载httpfs
INSTALL httpfs;
LOAD httpfs;
-- 2. 设置 MinIO 配置
SET s3_url_style='path';
SET s3_access_key_id = 's1BwDwDQ3ZOqxOexSW8z';
SET s3_secret_access_key = '1rbfLrg7Jv1TPcUL5t9xlHY7ugY74vSSBZKeAQjr';
SET s3_endpoint = '127.0.0.1:9000';
SET s3_use_ssl = false;


-- 3. 查询 Parquet 文件
SELECT * FROM read_parquet('s3://test/demo.parquet');
SELECT * FROM read_parquet('s3://test/demo.parquet') WHERE x=1;
SELECT * FROM read_csv_auto('s3://test/app_user.csv');
```

