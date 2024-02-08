# redis-academy

## Redis Data Structures 

Undestand Redis data types 

Core 

- String 
- Lists 
- Sets 
- Hashes 
- Sorted sets 
- Streams 
- Geospatial index 
- Bitmap 
- Bitfields 
- HyperLogLog


### Redis String 

Redis String are the most basic Redis data type, representing a sequences of bytes. Using **SET** and **GET** commands are the way we set and retrieve a string value. SET will replace any existing value already stored into the key, in the case that they key alredy existe.For instance we can store jpeg image inside a value. A value can not be bigger than 512 MB.
Documentation link : https://redis.io/docs/data-types/strings/

```
C#

var setBikeString = db.StringSet("bike:1", "Deimos")
Console.WriteLine(setBikeString) // true
var consoleWriteBikeValue = db.StringGet("bike:1")
Console.WriteLine(setBikeString) // Deimos
```

The **SEt** command has **options**. For example, I may ask SET to fail if the key already existes, or the oppsote, that it only succed if the key already existe.

```
 C#

 var responseFail = db.StringSet("bike:1", "bike", when: When:NotExists ); 
 Console.WriteLine(responseFail); // false
 Console.WriteLine(db.StringGet("bike:1")) // Deimos
 var responseSucceed = db.StringSet("bike:1", "bike", when: When:Exists ); 
 Console.WriteLine(responseSucceed) // true
```

There are number of commands for operating on String. For example if you wan to set new value to a key, returning the old value as the result you can use **GETSET**. this is useful if you want to keep track of new visitor on your website. 

Also it is very useful to set or retrieve the value of multiple keys in a single commands. This will reduce latency. For this reason there are the **MSET** **MGET** commands.

#### String as counters 

Now that we know String and that we know that String are the basic values of Redis, there are some other operation you can perform with the help of String such as atomic increment: 

```
C#

db.StringSet("task_completed", 0)
var incrementTaskList = db.StringIncrement("task_completed")
Console.WriteLine(incrementTaskList) // 1
var incrementNumbertask = db.StringIncrement("task_completed", 10);
Console.WriteLine(incrementNumberTask) // 11


```

### Redis Lists 

Redis list are linked list of string values. Redis lists are frequently used to: 

- IMplement stacks and queues 
- Build queue management for background worker systems.

#### Bisc commands 

- **LPUSH** addes a new elemtn to  the head of a list
- **RPUSH** addes a new elemtn to the tail  of a list
- **LPOP** removes and returns an element from the head of a list
- **RPOP** removes and returns an element from the tail of a list
- **LLEN** returns the lenght of a list
- **LMOVE** atomically moves alements from one list to another
- **LTRIM** reduces a list to the specified range of elements 

- Treat a list like a queue (first in, first out);

```
Node.js

const res1= await client.lPush("bikes:reapis", "bike:1");
console.log(res1); // 1

const res2 = await client.lPush("bikes:repairs", "bike:2")
console.log(res2) // 2

const res3 = await client.rPop("bikes:repais")
console.log(res3) // bike:1

const res4 = await client.rPop("bikes:repais");
console.log(res4) // bike:2


```

- Treaat a list like a stack (first in, last out);

```
const res5 = await client.lPush("bikes:repairs", "bike:1")
console.log(res5) // 1

const res6 = await client.lPush("bikes:repairs", "bike:2")
console.log(res6) // 2

const res7 = await client.lPop("bikes:repairs")
console.log(res7) // bike:2

const res8 = await client.lPop("bikes:repairs")
console.log(res8) // bike:1
```





