![webmongo](https://avatars1.githubusercontent.com/u/17017373?s=460&v=4)

# webmongo

## About us

Accessing server-side mongodb through client javascript API. This project is a branch of [dbcloud](https://github.com/csmake/dbcloud)

You can do almost invoke on mongodb through the javascript API in browser.

The client javascript api support `IE6.0+ Chrome FireFox and Wechat`

web: http://www.csmake.com

mail: rd@csmake.com   

## Start the basic project 

1. Run mongod first. 
2. Put dbcloud.war in your servlet server webapps ,Apache Tomcat8.0 , Glass Fish Server4.x and so on.
3. Open browser(Chrome FireFox IE) and put http://localhost:8080/dbcloud/, The address may be different depending on your settings

## Start to write your web application

1. Copy MongoCollection.java and MongoCollectionServlet.java from dbcloud.war /src/java/.. , into your src/java/org/dbcloud/mongodb

2. Copy all *.js files in dbcloud.war /js to your js directory. JQuery is necessary. IE6.0 need to use the 1.x version and json2.js is necessary.

3. Edit the web.xml, like this:

```xml

    <servlet>
        <servlet-name>MongoCollectionServlet</servlet-name>
        <servlet-class>org.dbcloud.mongodb.MongoCollectionServlet</servlet-class>
        <init-param>
            <param-name>dbhost</param-name>
            <param-value>127.0.0.1</param-value>
        </init-param>
        <init-param>
            <param-name>dbport</param-name>
            <param-value>27017</param-value>
        </init-param> 
        <init-param>
            <param-name>db</param-name>
            <param-value>test</param-value>
        </init-param>
        <init-param>
            <param-name>connection</param-name>
            <param-value>test</param-value>
        </init-param>
        <init-param>
            <param-name>user</param-name>
            <param-value></param-value>
        </init-param>
        <init-param>
            <param-name>password</param-name>
            <param-value></param-value>
        </init-param> 
    </servlet>
    <servlet-mapping>
        <servlet-name>MongoCollectionServlet</servlet-name>
        <url-pattern>/org.dbcloud.mongodb.MongoCollection</url-pattern>
    </servlet-mapping> 
```
4. Edit your html file, you can see the index.html 

```javascript
	﻿<!DOCTYPE html>
	<html>
		<head>
		</head>
		<body>
			<div id='log'></div>
		</body>
		<script src="js/json2.js" note='for IE6.0 ,old browser'></script>
		<script src="js/jQuery-1.12.4.min.js" note='for IE6.0 ,new version can use JQuery 2.x'></script>
		<script src="js/org.dbcloud.mongodb.MongoCollection.js"></script>
		<script>
			try {
				function log(msg) {
					$("#log").append("<p>" + msg + "</p>");
				}
				var table = new org.dbcloud.mongodb.MongoCollection();
				log("clear table");
				table.deleteMany({});
				log("insertOne and set options");
				table.insertOne({name: 'validation'}, {validation: true});
				log("count:" + table.count({index: {'$exists': true}}));
				var data = table.find({'index': {'$gt': 5}}, {projection: {'_id': 0}});
				log("find index > 5 and exclude _id column");
				log(JSON.stringify(data));
				log("replaceOne");
				table.replaceOne({name: 'dbcloud'}, {name: 'replace', status: 0});
				log("updateMany with options ");
				table.updateMany({'index': {'$gt': 10}}, {'$set': {name: 'updateMany', status: 100, index: 100}}, {upsert: true, validation: false});
				log("find all");
				var data = table.find();
				log(JSON.stringify(data));
				log('findOneAndUpdate');
				log(JSON.stringify(table.findOneAndUpdate({'index': {'$gt': 10}}, {'$set': {name: 'findOneAndUpdate'}, '$inc': {index: -1}})));
				table.close();
			} catch (e) {
				alert(e.message);
			}
		</script>
	</html>
```

