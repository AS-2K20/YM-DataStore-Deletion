<h1 align="center">DataStore Deletion:</h1>

> Using ```app.dataStore.delete()```, We can DELETE ONLY THE RECORDS OF A TABLE FOR THE GIVEN "_id".

To delete a record ( row ) from the table in the database, we need to know the **"_id"** Key of the Particular Record in the table.

We can find the **"_id"** key in the following ways : 

<h3>1. Using UI:</h3>

* After Logging into your account in https://app.yellowmessenger.com/, Go to your YM bot. 

* Click on __Database__ and then click on __Developer Tools__.

<img width="1435" alt="Screenshot 2021-04-20 at 6 42 48 PM" src="https://user-images.githubusercontent.com/66553883/115437130-6e21cd80-a229-11eb-9958-bf3f3e13271e.png">

* Click on the required Table and then click on __Run Query__ button.

<img width="1440" alt="Screenshot 2021-04-20 at 6 49 31 PM" src="https://user-images.githubusercontent.com/66553883/115437265-990c2180-a229-11eb-8787-44c95cf30d04.png">

* Scroll down and you can see the "**_id**" of each record in your selected table.

<img width="1440" alt="Screenshot 2021-04-20 at 8 03 13 PM" src="https://user-images.githubusercontent.com/66553883/115440448-5ba99300-a22d-11eb-81ee-68c19bda97e3.png">

<h3>2. Using Code:</h3>
	
Using __match all__ query in ```await app.dataStore.search()```, We can get the same output as above.

**<ins>EXAMPLE:</ins>**

```node js
async function dataStoreDelete(){
	let dbSearch = await app.dataStore.search({
	       table: "to_be_deleted",
	       body: {
		   "query": {
		       "match_all": {}
		   }
	       }
	   });
	app.log(dbSearch,"THIS IS DBSEARCH !");
}
dataStoreDelete();
``` 
	
**<ins>OUTPUT:</ins>**

<img width="935" alt="Screenshot 2021-04-20 at 8 24 59 PM" src="https://user-images.githubusercontent.com/66553883/115444248-f310e500-a231-11eb-97ea-6380d334da3e.png">
	
<h2>Deleting 1 Record:</h2>
	
Using ```app.dataStore.delete()``` , We can delete 1 record from the required table.

**<ins>SYNTAX:</ins>**

```node js
await app.dataStore.delete({ table: table_name, "_id": id }); 
```

**<ins>EXAMPLE:</ins>**

```node js
async function dataStoreDelete(){
	await app.dataStore.delete({ table: "to_be_deleted","_id": "607ebac8e644a142a75f4fa4"});
}
dataStoreDelete();
```
	
<h2>Deleting Multiple Records:</h2>

```app.dataStore.delete()``` can delete ONLY 1 record. So if we want to delete multiple records, we have to run this multiple times.

**<ins>EXAMPLE:</ins>**
```node js		
async function dataStoreDelete(){
	let dbSearch = await app.dataStore.search({
		table: "to_be_deleted",
		body: {
			"query": {
				"match_all": {}
			}
		}
	});
	app.log(dbSearch,"THIS IS DBSEARCH !");
	for(let columnId of dbSearch.hits.hits){
		await app.dataStore.delete({ table: "to_be_deleted", "_id": columnId["_id"] });
	}
}
dataStoreDelete();
``` 
	
 
**<ins>BRIEF EXPLANATION:</ins>**
 
1. Using ```app.dataStore.search()``` with a "__match_all__" query, we are getting all the records from the required table and storing it in a variable.

2. Using a For Loop, We are iterating the stored variable and getting the **"_id"** of each record of the given table.
 
3. We are running the above For Loop for all the records in the given table and we are using ```app.dataStore.delete()``` to delete all the records in the given table by iterating over the **"_id"** property of each record in the given table.

**<ins>NOTE:</ins>**
 
* I am not sure whether ```app.dataStore.delete()``` is an Asynchronous or a Synchronous cloud function. 
 
* Just to be on a safer side, I am __assuming__ that __it is an Asynchronous cloud function__ and thus I am using ```await``` keyword for it.
