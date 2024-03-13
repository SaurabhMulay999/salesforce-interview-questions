### Salesforce Security Model
<hr>

**Explain Salesforce Security Model: With Examples and Theory.**

Who sees what in SF....!!!!!!!!!

 The bare use of the security model is to avoid unauthorised access to the application or to our data. It is divided into following types we can say.
  
  1.UserName password (need username and pass to login)
  
  2.Login IP based security (you must be within the ip range to access data)
  
  3.Login hrs (u can set the hrs: example user/admin can only login within hrs)

  ### **Types of securities:**
  
  1.Object level (Accessability) Based on CRED settings how can u access the Table (In term of DB)
  
  2.record level
  
  3.field level

  **Explanation:**
  
  Object level security can be provided by profiles and permission sets and permissions sets are used to give addional access.
  Record level security can be defined as visibiliity, how many record user can see. it can be implemented in two steps. the first step is to
  lock all access using OWD.(Organisation wide default).We are restricting the access or visibility of table. the access levels are "Public read-write" (you and    others can see your records), "Read write transfer" (this available in case and leads because u can transfer the ownership), "public read only" (u can only 
  read   others record), private (pvt is most restricted access as u can only see u'r records), "Controlled by parent" (it is available in case of M-d 
  relationship).
  the second step is to open up the access on record level. how to open up the access, there are ways by using sharing rules (criteria based and owner 
  based),Manual sharing, Apex sharing (dynamic sharing), by using Teams. thease are comman ways to open up the access on record level. FLS (field level security)   can be implemented on profile level or can be on field also.

  ### **Object level Security:**
  <hr>

  It can be implemented using profiles and permission sets. when u have multiple users and in db's table they need different diff access in term of CRED (crete, 
  read,edit, delete). so for the users you can have unique profile, profile is set of access priviledge and settings given to the user. Every user has profile
  associated with it. one user one profile and basic accessability of a user is dependant on the profile. one profile can be associated with many users. (one to 
  many rel). System admin is the profile with all accesses. User: license is not consumed on creation of profile but consumed on creation of users associated 
  with that profile. free dev org has only 5mb space. business's need more so option is to buy the license to have more users and space. Most of MNC's used 
  unlimited version. per users may be costs 300$. feature licens also available with salesforce. Features can be accessable but need to buy it, suppose you need
  sales cloud, service cloud, marketing cloud. those features are need to buy.
  But the issue is onward they have faced...suppose profile p1 have CR access and profile p2 has CRE access but now for temporary purpose they need CRED access
  now the solution is either use profile p3 but that is not good solution, providing it to existing profile will open up cred permissions to all the users but
  that's also not desirable. So permission set can be used to give addtional access. basic access can be provided by profile and now permission set will provide
  addtional access to the user.fls can be handled through profiles and permission sets. Dont clone syatem admin profile to create new profile. Objects permissions
  are not editable on sys admin profile.So conclusion is the basic access of user came from profile and addtional access is given by permission set. Total access 
  to the user is nothing but profile+Permission sets.

  Interview Questions:

  1.Profile vs Permission sets??
  
  2.profiles vs Role???
  
  3.Types of Salesforce security?
  
  4.precedence of securities??

  5.significance of profile, permission set, owd, sharing setting.

  6.Difference between sharing rules vs manual sharing. (In sharing rule ,one rule can applied to manay records)

  **These are some comman questions were asked in interviews:** (Covered in next topics)


  ### Record Level Security:
  <hr>

  Record level security is all about visibility. visibility of table or records we can say. We implement it in two step process. first is to locking up the access
  and second one is to open it up. owd is the first step to lock the access (default access). {accesses: ["public read/write trasfer", "public read/write default 
  setting)","public read only" ,"private" ] }. private is most restrictive one."Controlled by parent" access is nothing but the child which id m-d with parent 
  that child is owned by parent only, child is not owned by anyone then. To open up the access we have sharing rules. sharing rules are  automated exceptions to  the owd for the specific group of users so they can get the records that they don't own or not see. We have criteria based sharing rule and owner base sharing
  rules.

  Criteria based: rules shared records based on some criteria, like Type=='Customer' then read access to set of users likewise.

  Owner based: rules are shsring records based on owners.. ((role1..users))----> ((role2..users)) so we are just sharing records which are visible to role 1 to
  role2. that is owner based sharing rules and at same time we can provide the level of access to the records. we can do it for multiple records. but if u want 
  to go by record by record and if the object's owd is private then above the record u will get option of manual share. So Individually u can add sharing. the 
  third way is to implement using apex. And another option is to use Teams where u can do account bases sharing, for diff diff customer diff rules likewise.
  Role Hierarch,(Grant access using hierarchy:option available in OWD settings) if the option grant access is true then data will be shared in hierarchy. (your boss can see your records).
  sharing records using Apex:
  <a href="https://developer.salesforce.com/docs/atlas.en-us.apexcode.meta/apexcode/apex_bulk_sharing_creating_with_apex.htm">Sharing using apex</a>
  
  ##### difference betn Profile and Role: 
  profiles are not in hierarchy while roles are in hierarchy. Profile is mandatory while role is not. checkout the other differences as well.

  ** Notes:** 
  
  1.when changing the objects owd to private, the salesforce initiate the internal process and it will create a object OBJECT__sharing and then it'll set
  owd to private. 
  2. profile is responsible to accessability of record while role is responsible for visibility of record.
  3.permissions sets aint affect visibility. 
  
  
  ### SoQL 
  <hr>

  possible scenarios and queries:

  Parent to child query: (Subquery) 
  Account-parent contact-child

  **Example:**
  Select Id, Name, (select id, fname, lname from Contacts) From Account where Id=''
  

  Child to parent Query: (Every child has the ref var to the parent)

  **Example:**
   Select Id,Fname, Lname, Email, Account.Id, Account.Name from Contact where AccountId=''
  
   #### Scenarios:
  1.Getting all Accounts that has 0 contacts associated with it
   ```
   Select Id, Name from Account where Id Not In (Select AccountId from Contact)
   ```
  2. Get kth opportunity Record that has max Amount. (Assume k given)
  ````
  Select Id, Name, Amount From Opportunity Order by Amount DESC LIMIT 1 OFFSET k
  ````
  3.Example with TypeOf and When/Then clause.
  ```sql
   SELECT 
    TYPEOF What
        WHEN Account THEN Phone
        ELSE Name
    END
  FROM Event WHERE CreatedById IN ( SELECT CreatedById FROM Case )

1.TYPEOF isn’t allowed in queries that don’t return objects, such as COUNT() .

2.TYPEOF expressions can’t be nested. For example, you can’t use TYPEOF inside the WHEN clause of another TYPEOF expression.

3.TYPEOF isn’t allowed in the SELECT clause of a semi-join query (semi join queries are where we use IN, NOT in operator). You can use TYPEOF in the SELECT clause of an outer query that contains semi-join queries (In the above example we have used it in outer select). The 6.following example is not valid because TYPEOF is used in the semi-join query:

  ```
4. GROUP by ROllUP:
    
```` sql
SELECT LeadSource, COUNT(Name) cnt FROM Lead GROUP BY ROLLUP(LeadSource)

Returned Aggregated results include an extra row with a null value for LeadSource that gives a grand total for all the groupings. Since there is only one 
rollup field, there are no other subtotals.

`````
5.Filtering soql queries With SECURITY_ENFORCED (Must asked Interview Question)
``` js
 [Saleforce docs said] Use the WITH SECURITY_ENFORCED clause to enable field- and object-level security permissions checking for SOQL SELECT queries in Apex code, including subqueries and cross-object relationships.
Apex generally runs in system context; that is, the current user's permissions and field-level security aren’t taken into account during code execution. Sharing rules, however, are not always bypassed: the class must be declared with the without sharing keyword in order to ensure that sharing rules are not enforced.

List<Account> act1 = [SELECT Id, (SELECT LastName FROM Contacts)
   FROM Account WHERE Name like 'Acme' WITH SECURITY_ENFORCED]
if the user has field access for LastName, this query returns Id and LastName for the Acme account entry.

List<Account> act1 = [SELECT Id, (SELECT LastName FROM Contacts), 
   (SELECT Description FROM Opportunities)
   FROM Account WITH SECURITY_ENFORCED]

If field access for either LastName or Description is hidden, this query throws an exception indicating insufficient permissions.
```

### lightning web components
<hr>
Prerequisite: Javascript (Find the Notes: <a href="https://github.com/SaurabhMulay999/SF_InterviewQuestions/blob/main/Course%20On%20JavaScript-_Jonas-1.pdf">Javascript Notes</a>)

Decorators:

**Public properties**:: 
The public properties are decorated using @api and they are also reactive, when the value of property changes it results redering component and invoking rendercallback() hook. Though they are reactive but Js playes very important role here,

consider below scenarios::
```js
'===' operator in js compare data types as well as the values, it also called strict equality operator.

'1'==='1' ==>true
'str'===new String('str') ===>false (becuase string!=Object //type mismatch)

In case of primitives it's okay...that new values has been assigned with same type ,
now just render the component.
But about non primitive data types like objects, arrays:

const obj1={x:10};
const obj2={x:10};

obj1===obj2 ===>false.  (Why, they hold different ref in the memory space and data type is same but still it has no mactch)
if u try to assign the referance;

const obj3=obj1;
obj1===obj3 ===> true.
```

and there lwc fucked up!!! reactive properties ain't reactive in case of the objects , arrays, custom data structures.
as per their docs they suggested: [[When manipulating complex types like objects and arrays, you must create a new object and assign it to the field for the change to be detected.]] and that is kind of bad idea.

So maybe they have accepted the mistake and come up with @track decorators. which tracks the mutations in the objects so maybe that's why @track...becuase it
tracks the changes.
[[docs: However, the framework doesn't observe mutations made to complex objects, such as objects inheriting from Object, class instances, Date, Set, or Map.]]

Note when u'r using @track for objects:

If a object's property contains an object, to track changes on the object's properties, annotate the property with @track. 

example:
```js
@track const obj={
 @track prop:{
  X:10;
 }
}
```

Note while using @track :; dding a new property to obj or a change doesn’t trigger a rerendering.

example:

it's just js habits to declare he variables with var and let but in classes you don't have to do that.
````js
@track const obj={val1:"saurabh"}
obj.val1="sarah" //will trigger the rendering

but:
obj.val2="saurabh" //won't be triggering the rendering

````

so again they fucked up over here, Likw kindoff forgot to handle the edge cases so they have suggested as per the docs:
[[To rerender your component when adding a new property, assign the object to a new object with both values.]]

like:
```js
@track obj={val1:"saurabh"}

obj={
...obj,
val2:'sarah'
}

now this will render the component: 

```
But story of @track ain't over...They again fuked up with reactivity, LWC won;t able to track reactivity of complex objects:

1.Date ::  lwc won't able to track changes in date obj.
so they again suggested in the case of dates:

reassign he new date to that reactive property to make it reactive...
```js
like below code they have cloned the date , modified it and assiged it back to reactive property
updateDate() {
  const cloned = new Date(this.x.getTime());
  cloned.setHours(7);

  // Assign the new date instance to rerender the component.
  this.x = cloned;
}

```
 Now that's all...enough on @api and @track.

 #### Shadow DOM:

Shadow DOM is a standard that encapsulates the internal document object model (DOM) structure of a web component. Encapsulating the DOM gives developers the ability to share a component and protect the component from being manipulated by arbitrary HTML, CSS, and JavaScript. The internal DOM structure is called the shadow tree. The shadow tree affects how you work with CSS, events, and the DOM.

Since not all browsers implement Shadow DOM, LWC uses a synthetic shadow polyfill for Lightning Experience and Experience Cloud. A polyfill is code that allows a feature to work in a web browser.Shadow DOM allows hidden DOM trees to be attached to elements in the regular DOM tree – this shadow DOM tree starts with a shadow root, underneath which you can attach any element, in the same way as the normal DOM

#### lifecycle hooks:

constructor(): This is where the component is initialized. You can set default values and perform one-time setup.

connectedCallback(): After the component is added to the DOM, this hook runs. It’s a great place to perform DOM manipulations and data retrieval.

renderedCallback(): This hook is triggered after rendering occurs. It’s ideal for operations that require knowledge of the rendered DOM.

disconnectedCallback(): When the component is removed from the DOM, this hook is invoked. Use it for cleanup operations and resource releases.

errorCallback(): If an error occurs during rendering, this hook is called. It’s your opportunity to gracefully handle errors.


![image](https://github.com/SaurabhMulay999/SF_InterviewQuestions/assets/90036775/42820bc5-6e2d-4d11-966e-4d4ec7957430)


  #### component communication:

  1.Passing data  from parent to child:
  
  2.passing data from child to parent:

**1.Parent to child:**

````js
just create a public property using @api; in a child component

@api prop;

and passs it as prop in the child component from parent:
<c-child prop="hell"></c-child>
so when u pass the  it down the property prop will catch data from parent automatically.

you can also able to call childs's methods from parent but the methods also needed to decorate with
@api.
````

**2.child to parent**::

communication in lwc is one way as you can only send data from parent to child, but reverse way is also possible.
phenomeno also called as lifting the state up.

how to acheive::

1.either u can pass data on click of some action from child.
````js
child:
<button onclick={click handler}></button>

msg="saurabh"
clickhandler(e){
 this.dispatchevent(new customevent('sendevent',{
     detail: this.msg;
        })
}

parent:
As you named the event in child as sendevent...but you can register that name in parent only

<c-child onsendevent={eventhandler} >

here u need to mention "on" in front of custom event , from child we are passing data in sendevent.
to catch it, it'll be catched in eventhandler: e.details
eventhandler(e){
 console.log(e.details); //this will print saurabh
}

````

**We'll learn Pub-Sub / event emmmiter pattern in detials afterward...**

### Async Apex:
<hr>

1.Future Method          

2.Batch Apex

3.Queueable Apex

4.Schedulable Apex

Synchronous vs Async nature of programming Languages:: Parallel processing is possible in case of no dependancies between two processes. so we run the non dependant process on a saparate thread.In apex, async apex provides higher Governer limits than sync apex (example: in sync apex 100 soql calls but in async limit is 200). But:: Number of Async request in 24 hrs is also an governer Limit. So Inki shakti ka galat istamal nahi ho sakta!!!!! ...:> 
When you have to divide the code in different threads then use future Methods. When u have to define logic in queue format, (as queue data structure, First in First out).and you have to chain the jobs like one after another and for every job there is saparate governer Limits. So every task/job in queue run in different transaction.So scope of Governer limit is high in case of queueable apex. Batch Apex is to handle millions of records converting it into small batches or chunks and every batch has it's own governer limits. And schedule apex is to run the peice of code at perticular defined time, we can acheive this using schedule apex.

**1.FUTURE APEX:**

Future methods are used to run processes on saparate thread, at the later time **when system resources are available.** future method runs when system resources are available so to explain with example.We have setup objects and non setup objects.example: setup objects are like 'User', 'Profile', 'Permission sets' and non setup are comman objects like 'account', 'contacts' etc. **So salesforce says you cannot handle both setup and non setup objects in single or same transaction.** if you have tried inserting user record and account record in same transaction, **You'll get Mixed DML exception.** So solution is to use Future Method,Run both insert operations on saparate threads to avoid Mixed DML operation.

```js
Code:

global class Pokemon{

@future  ---> this annotation is mandatory to define method as future

public static void goPokemon(list recordids){  //return type must be always and void and method must be static in case of future method

//future method only accept primitive data types and collection of primitive data types.
pass; //process your logic here
}
}
```
**To be Noted on Future Methods:**

1. annotate the method with @future, if doing callouts make sure write @future (Callouts=enabled/true)
2. future methods only accept primitive data types as arguments to function.  (Salesforce introduced Queueable apex to overcome this issue : it can accept non primitive dtypes)
3. future method used to avoid mixed dml exceptions.
4. It provides higher governer limits
5. it helps in webservice callouts.
6. Does future methods are useful to avoid Timeout Exceptions which occure if callout takes more time than expected????

On point no. 6::: It may or may not. but it won't guarante that it will be usefult to avoid timeout exceptions. but it can help in offloading long-running processes to asynchronous execution, which might indirectly contribute to avoiding certain timeout issues.

7. One future method cannot call another future method. If required to do so use Queueable Apex.So that u can able to chain the jobs.

**Best Practices:::**
1.ensure that future method execute fast as possible. So try to write minimal logic inside the method.

2.Test future methods bulkily.

3.to handle larger data then use batch apex than future methods.

4.No more than 50 methods call per apex invocation. (it is a governer limits).So system wont allow more than 50 active future methods,so it's kind of governer limit.

5.[IMP] The maximum Number of future method invocations per 24 hrs period is 250,000 or Number of user license* 200. This limit is for your entire org and is shared with all asynchronus Apex calls like batch apex, queueable apex, schedule apex and future method.All the work after limit breach will get rolled back or simple salesforce won't be doing that. If u want to increase the limit then Pay for more user license.

````js
Example of future method:

public class pokemon{
 
 public static void usePokemon(){
     Account a= new Account(Name='Sarah');
     insert a;
     util.callFuture();
 }
}
class util{
  @future
  public static void callFuture(){
     Profile p= [select id from Profile limit 1];
     user u=new user(name='Pikachu',Profile=p.id);
     insert u;
  }
}
````

So now just to avoid mixedDMLException we are running both insert operations on different different threads.

#### Queueable Apex:

1.Its a advance version of future methods. quueueable apex allow non primitive data as argument.

2.It's simple called by System.enqueuJob(). 

3.Interface which we are goin to implement is queueable interface,

4.This queueable apex has simplicity of future methods and power of Batch Apex.

**Advantages**

1. It allows non-primitive Dtypes.
2. When u submit your job call system.enqueueJob method. the method returns the id of asyncapex job record. You can use that ID to identify your job progress.
3. Chaining jobs is possible. You can start another job from the current running job, so sequential processing is possible. 

````js
Example:

public class AccountUpdate implements Queueable{

    private List<Account>accounts;
    private ID Parent;

    public AccountUpdate(list<account> records,ID id){
         this.accounts=records;
         this.Parent=id;
    }

    public void execute(QueueableContext context){
        for(Account a:accounts){
            a.parentId=Parent;
         }
       
       update accounts;
       System.enqueuJob(new Job()); //chaining the job
    }
}

Queueable logics aren't that simple. it's just a exmple with min code;

To run method:
create a instance of class and pass it into enqueuJobs():
ex:
AccountUpdate ac=new AccountUpdate(accounts,Parentid);
Id jobId=System.enqueuJob(ac);

````
when there are millions of records the queueable apex helps.

1.The execution of queue job count once against the shared limit (2.5lakh) for async apex method executions.

2.You can add upto 50 jobs to queue with system.enqueuejob in single transaction. to check how many queueable jobs have been added in one transaction, call Limits.getQueueableJobs();

3.There is no limit for depth of jobs, (It ain't recursion:) inf chaining possible but Parallely you can run only 5 jobs. 


#### Batch Apex:

1.Batch apex is used to run large jobs (in millions of records) that may exceed normal processing limits.

2.Using Batch Apex you can process records asynchronously in batches. Every batch has its own governer limits. Max we can query 50 Million records.

3.Methods to implement in batch apex: 1.Start 2.execute 3.finish

4.start method used to fetch the records, in 'execute' method business logic runs and in finish method we can able to write post processing logics.

**Example**:

```js
global class UpdateAcc implements Database.Batchable<sObject>{
  
  global Database.QueryLocator;
  
  start(Database.BacheableContext bc){
    string q='select id,Name from account';
    return Database.getQueryLocator(q);
  }
  global void execute(Database.BacheableContext bc, List<Account>scp){
    for(Account a: scp){
      a.Name+='Updated by batch class';
    }
    
    update scp;
  }
  global void finish(Database.BacheableContext bc){
    //post processing logic
  }
  
}
```
In batch class we have to implement the interface Database.Bachable.

To run the above methods or class:
```js
UpdateAcc acc=new UpdateAcc();
Id jobId=Database.executeBatch(acc);
//pass the instance of class in executebatch
```

If in scenario, wanted to reduce batch size that can be acheivable by:

**Why to reduce Bacth Size????**

=> Example, suppose you have 2 records : one has only 3 fields filled in (minimum data) and second record has upto 50 fields filled in.
Now in the memory the second instance or record would weigh higer than the first one. So it is not recommanded but a good practice to reduce the batch size when you have weighted data or records, or we can say heavy records.

```js
UpdateAcc uac=new UpdateAcc();
Id jobId=Database.executeBatch(uac,100);

//here we kept batch size 100
```

**Governer Limits::**

SOQl Query::  Normal Context: 100 Soql per transaction,  Batch Context: 200 Soql Per transaction

Records Retrieve: Normal Context: 50,000, Batch Context:50 Million records

Executed code statement: Normal context:6M characters (Total in org)  Batch context: 12M characters

Heap Size: Normal Context:6Mb, Batch or async Context: 12Mb


#### Schedulable Apex:

1. You can delay the execution so that you can run apex classes at the specified time.
2. it is also possible to invoke batch apex using schedule apex.
3. The class or method will start executing at that respective time, but sf won't guarantee that schedule apex will invoke exactly at a defined time.
4. Actual execution time maybe delayed based on resource or service availability.
5. You can only have 100 scheduled Apex jobs at one time.

Example: Code
```js
global class Reminder implements Schedulable{
  
  global void execute(SchedulableContext st){
    list<account> accs=[select id,name,email from account limit 5];
    //assume sendEmail implementation: to send emails of accouts
    Utis.sendEmail(accs);
    
  }
}
```
**How to schedule the job:**
Setup ==> Apex classes => Schedule Apex ==> Job Name ==> Time ==> Successfully Scheduled a Job.

To track:
Apex Jobs ==> It helps you to monitor the job.

**NoTes on Above topic::**

1. Most of the interview questions were asked about async Apex are related to Some Business logic and interfaces and Most of the questions were asked on governor limits.
2. The interviewer won't be going to tell you to implement the scenarios but well these topics must be conceptually strong to crack the interview.


### Exceptions::
<hr>

1. Exceptions are Runtime errors which stop the execution of a program.
2. DML Exceptions: A DMLException is thrown when DML operation fails.
3. QueryException: When query statement fails to execute.
4. sObject Exception: It throws when you work on some object and it fails. (Writing text in number field or Making required field Null)
5. listException: When a Problem occurs with a list or list of sObjects then this throw.
6. NullPointerException: When u  dereference null object. Ex( Account a; , a.Name='saurabh' ) will throw nullptr
7. MathException: When a mathematical operation fails.
8. TypeException: when there is data type mismatch.(or when typecasting was not right)
9. Limit Exception: When the resource limit has exceeded then this limit exceptions are thrown.(when resource hit governer limits)
10. JSONException : When there are error while parsing a JSON string this exception thrown.
    

````js
//DMLEXception
Account a=new Account();
insert a;  //will throw DMLException. (You're trying to insert account with no mandatory field filled in)

//QueryException
Account a=[select id,name from account where name='Sarah'];
//If no record found it'll throw query Exception : List has no rows...

//listException
list<string> str=new list<string>();
str.add('Saurabh');
str.add('sarah');
system.debug(str[1]); //sarah
system.debug(str[2]); //Element wont exist so will throw ListException.

//NullPointerException
Account a;
a.name='Saurabh';  //this will throw the null pointer exception, because we have not called the constructor above.
insert a;

//Limit Exception:
for(Account a: [select id From account]){  //or just try a kind of infinite for loop and it will throw CPU time limit exceeded.
    a.Name+='updated';
    insert a;
} 
````

**Interview Tips and Questions on Exceptions::**
1. The interviewer mostly asked questions on best practices and the exceptions that we faced during project tenure.
2. use try catch to handle exceptions.
3. You have multiple abstract methods associated with the exception object. Example catch(Exception e){ debug(e.getTypeName(), e.getMessage(), e.getCause(), e.getLineNumber(), e.getStackTraceString()) }


### SalesForce REST API:
<hr>

REST: Representational State Transfer. (Protocol is stateless).
It may be less secure than Soap API.

Statelessness:: meaning each request from a client contains all the information needed to understand and process the request.
The server does not store any information about the client between requests.

Suppose there is an Integration Between Salesforce and Github::

We can say every time a new account is created in Salesforce that would call Github API to create its users. So here salesforce acts as a source. This will be called **Outbound** calls.
If GitHub hits/consumes salesforce then it would be **inbound** calls. (here if you're from the salesforce side then consume would be the better choice of word and if you are Github team then you can use 'hits').

We can able to implement HTTP methods: GET, POST, PATCH, DELETE, and PUT. Because RESTful services are commonly implemented over HTTP.
The PATCH method is used to apply partial modifications to a resource. It is typically used when you want to update only a portion of the resource's data, rather than replacing the entire resource.

Notes:
1. When you have to mention some extra logic, and calculation then implement a custom API or else use the standard one that Salesforce provides. 
2. Access Modifiers in Salesforce::

private: Visible only within the same class.

public: Visible in the same Salesforce namespace.

global: Visible in any Salesforce org, including external systems making RESTful requests.

Consume an API::

```js
//blob: binary large Object data type.

//RestContext has two props 'request' and 'response';
//So when 3rd party user has requested data/request has put into RestContext.request

RestResource(urlMapping='/CreatePokemon')
global class Restful{
@HttpPost
global string string PostMethod(){
    RestRequest req=RestContext.request;
    RestResponse res=RestContext.response;
    Blob bdy=req.requestBody;

    return res.toString();
} 

@HttpGet
global static sObject GetMethod(){}

@HttpPut
global static string PutMethod(){}

}
```

### Integration (Salesforce):
<hr>

Considering data formatting, 
**Are JSON and JS Objects are same??**

==> JSON is a language-agnostic data-interchange format.

The syntax of JSON was inspired by the JavaScript Object Literal notation, but there are differences between them.

For example, in JSON all keys must be quoted, while in object literals this is not necessary:
```js
// JSON:
{ "foo": "bar" }

// Object literal:
var o = { foo: "bar" };

The data types in JSON are also restricted to the following values:
1.string
2.number
3.object
4.array
```

The use of integration is to connect two different modules or systems together so they can interact and share the data in between. To perform integration salesforce provides different tools.
**At low code level:** MileSoft Composer

**Low Code+Code:** Platform events, Change Data Capture, Salesforce flow, Salesforce Connect, External Services.

**Dev Tools:** Apex, Platform APIs, Heroku connect, Mulesoft Anypoint Platform.

There is a lot to be added, wait!¡¡¡¡¡¡¡¡¡¡¡¡¡¡¡¡¡¡¡¡¡¡¡

### **Let Cover some backend::**(Complete Optional): Node js 
<hr>

Js can be used with backends. we use its runtime Example Node, browsers, Bun (Written in Zig). The best example of frontend + backend is the chatGpt, Open API has a close source model so they never going to run the AI model on your machine/browser. So openAI server is Backend, Some code runs for the input you have given to Message box.

Http Server: 
Https primarily a protocol define how machines will goin to interact with each other. Http server exposes the service to the world. Like it accepts input from the user and after some checks it'll respond or send the response to the client. 

```js
function init(input:string){
    return response;
}
```

So directly or indirectly you as a client hiting some one's functions or code. And server respond back with some data well.

Client needs to know about the endpoints, The auth flow, Header, body, queryParams, Method (POST,GET,PATCH,PUT) and whether it hosted on https or http protocol. and when server receive the request it must have respond with response body, status code, respone headers. 

**URL:**
**https://** ---> is protocol.

**google.com**--> is the url.

**/fetch/github/users** ---> is the Routes.


Why do we need status codes and the headers if we have response body?? Because https is protocol...we have to follow anyway. It's a set of rules. 

So google is just a domain Name behind that every domain or DN has Assigned to an IP. try **ping google.com** and you'll get and IP. behind the scene DNS do it for you , 

To create an Http server:
```js
const express=require('express');
const port=3000;

const app=express();

app.get('/', (req,res)=>{
  res.send('Hi sarah');
})

app.listen(port,function(){
    console.log('Sever running on port 3000');
},);

```
Two apps cannot access the same port. So only one process can be active on only port. The issue with the express or to say it ain't recognize the body. request body so need to used the 3rd party module
body-parser.

npm i body-parser
```js
const app=express();
const bodyParser=require("body-parser"):

//syntax to use it
app.use(bodyParser.json())

app.get('/', (req,res)=>{
   console.log(req.body);
})
```

So not if you hit the port, it'll show you the request body. 

#### basics of Express:

Middlewares:

Prechecks are required when someone anonymous hit your server, The authentication is required withought that you are not that secure so through the middleware you can authorise the user first. So user can pass the creds through the query params, request body or the headers. We can write the logic in middlewere.

```js
(With No MiddlewareS)

const express=require(express);

const app=express();

const db={};

app.get('/',function(req, res){

    //writing the whole logic here only (no middleware)
    //assuming user is passing username and pass in query params
    const uName=req.query.username;
    const pass=req.query.password;

    if(db[uName]==undefined ||  (db[uName]!=undefined && db[uName]!=pass)){
        res.status(403).json({
            msg:"User ain't found"
        });
        // here directly we return if user ain't found with valid response and the code
        return;
    }

    //rest of your logic to do the things once user is authenticated.
    
})


```
With the middlewares: (Moving code to saparate functions to Stay on DRY principle)

```js
const express=require('express');

const app=express();

const db={};

//inside app.get we can give the order of execution of the middlewares. here usernamevalidation
 will run first then the callbackfunction.

//app.get take range of callback functions, at last the control reached to last callback.

//next() is just a iterator to call next callback function

app.get('/', usernamevalidation, function(req, res){
    res.statusCode=200;
    res.json({'msg':'Successfully created user and authenticated'});
})

function usernamevalidation(req, res,next){

    const uName=req.query.username;
    const pass=req.query.password;
    if(db[uName]==undefined){
        //create user:
        db[uName]=pass;
        console.log(db);
        res.status(200).json({'msg':'user created successfully'});
        return;
    }
    if((db[uName]!=undefined && db[uName]!=pass)){
        res.status(403).json({
            msg:"User ain't found"
        });

        return;
    }

    //it will execute the next middleware
    next(); 
}

const PORT=3000;
app.listen(PORT,function(){
    console.log('Server activated');
});

```

next() function will be accepted a callback to the middleware function with request and response, but the next() will be called once we passed the criteria or in real world we can say authentication then we can exexute the next middleware or callback. this implementation follows the DRY principle. (DO not repeat).

what is app.use(express.json())

--> use mean this middleware goin to get called every where.

define a middleware, if i do know our middleware is goin to get called every where in every route. Then just """use""" it.
for example:

```js
const express=require('express');

const app=express();

const db={};

let numberofReq=0;
function threshold(req,res,next){
    numberofReq++;
    next();
}

app.use(threshold);
```

so now any route under this app.use(threshhold) have threshhold as a middleware. but there in middleware you have to call next() in order to hand over the control to next middleware which is part of the route. 
why body aint directly get accessed by req.body??? becuase body can be any format json, text , html so you have to parse it first inorder to use it. 

**Input validations in request Body & Catching errors globally: **

--> **Global Error catch:**

if there is the error at any route you can use app.use at last of the routes so that if any error hit any route globally it'll shows the error response, the function will accept error as an argument also.
and work same as middleware.
```js
const express=require('express');

const app=express();

const db={};

let numberofReq=0;
function threshold(req,res,next){
    numberofReq++;
    console.log(numberofReq);
    next();
}

app.use(threshold);

app.get('/', usernamevalidation, function(req, res){
    res.statusCode=200;
    res.json({'msg':'Successfully created user and authenticated'});
})

function usernamevalidation(req, res,next){
    const uName=req.query.username;
    const pass=req.query.password;
    console.log(nulls.keys)
    if(db[uName]==undefined){
        db[uName]=pass;
        console.log(db);
        res.status(200).json({'msg':'user created successfully'});
        return;
    }
    if((db[uName]!=undefined && db[uName]!=pass)){
        res.status(403).json({
            msg:"User ain't found"
        });

        return;
    }
    next(); 
}

//it'll catch the error for global usecase it'll run for every route at last

app.use(function(err,req,res,next){
    console.log('Error');
    res.json({'Msg':'Error'})
})

const PORT=3000;
app.listen(PORT,function(){
    console.log('Server activated');
});

```

ZOD is used to validate the input in runtime. Checkout the zod's documentation. below example if we have to validate the input Username suppose we expect username as in numbers only so we enforce
it using zod and parseSafe method simply check if the input from user is number or not , if not then it'll set the response (variable defined in code :see below) .success to false else true. and as per the success variable you can tell the enduser that the input he has provided is not the correct type.

Example:

```js
const express=require('express');
const app=express();
const z=require('zod');
const schema=z.number();

app.get('/', function(req, res){
    const uname=req.query.username;
    const response=schema.safeParse(uname);
    if(!response.success){
        return InputInvalid(res);
    }
    res.send({response});
})

function InputInvalid(res){
    res.send({'msg':'input is invalid'})
}

```
you have to define a schema of how your input from user look a like. Scehma for username and password.

```js
const z=require('zod');

const schema=z.object({
    email:z.string().email(),
    password:z.string().min(10)
})

```

### Authentications:
<br>

you dont always expect user to provide the username and password, suppose you logged in on linkdn it won't ask you the username/email and password every time. So it it very numb way to asked user the username and pass. 
the better way is to provide a token to a user and ask the user to send back the token in all future requests and if the user has logged out then remove the assigned token from or revoke the token assigned to user from you db or backend.

First time when your are login or user is login he'll provide the username and password, respond to that server will return a Token (After validating the user). Now we'll store it in storage (browser storage) [local storage]. Now every request onwards the user is sending user will send the token along , so it won't check the username and pass it'll just check the token everytime. Storing email and password in local storage is fucking bad way!!!! 

Basic authentication: we want to restrict the api for the set of users or only allow to hit if user is authenticated.
Hashing is important over here to hash users username + password in an anonymous auth code. becuase if user is loggin in with username and password, the backend will store the password somewhere in database. But it's always risky to store the passsword in plain. So it get hashed. So directly it ain't store, Why to hash the passwords:" Security purpose "  and also maybe the multiple users have same password So hashing helps. And the server or db owner ain't able to check the passwords of the users. and they are not able to decrypt it also. Hashing is one way and encryption is two way, you can decrypt the encoded data with key or security token.

**JSON web Tokens:** 

1. JSON: It take some json input and give you some structured or long string, (A formatted string) It differs from hasing and encryption.
2. Token: and it takes the input and gives you a Token.

   JWT: About jwt if you provide an input it gives you an output (string) and who ever has that output string can able to see what is an input (This is kind of weird: it ain't hashed or protected). It just convert complex objects in a long string. and anyone has that long string can able to decode that object. For example on any website if you open a n/w tab (Already logged in) and checked headers of your api request. It'll show authorization "Bearer" <TOken>. That token is actually JWT. If I copy that jwt token and visit any client that convert jwt token to valid objects, it'll convert your token to show you payload or your data. (Email). JWT token has 3 sections or parts, Overall token comprises Header, payload, Signature.
   Suppose you password has hashed and put into the db and  your information has already there in term of the jwt token, the jwt token + password has parsed into a function again to verify your identity. JWT.verify. So in case of GPT take an example:
   Suppose you have provided an input json data:

   { 
      email:"sarah@kc.com",       ====>( JWT Encoded )
      pass:"ede@123"              ====>(             )
   }

   Out of json data it'll generate the Jwt token, but it ain't everything. suppose you hit a server with some payload or request, It'll bring back your hashed password from the database. and it'll parse both jwt token and hashed password in JWT.verify() function (or abstract code) and it'll allow only if that matches. or verified.

  ( JWT(Token)                 )
  ( &&                         )    ======> JWT.verify()  ====> Allowed if verified.
  ( Hashed password (From DB)  )


  From verify they may extract out our orignal info and matched any email assocaited with password (But not sure if they are doing the same or not) to just verify that it's you only that hiting the server.
  Before we have discussed to store the tokens in browser storage so every time user don't have to login. So in the local storage of browser it store it. Local storage ain't removed or clear if you closed the browser, or you restarted the machine also, it'll be stored always there. while session storages are temporary. 

  Build simple authenticaltion:

  **POST request** to Sign in route: With user input {Username:"", Password:""} return a user Json web token (No password included : JWT ain't contain the users password,Store it else where or in db) with the username encrypted. 
  
  **GET request:** return a number of items in menu if and only if the current user is signed in.(Token which provided to user is correct)

If any case user has logged out from the app then from local storage revoke that token but storig that token and revoking can be handle by frontend.

 Code:

```js
const express=require('express');

const app=express();
const jwt=require('jsonwebtoken');

//jwtpass is a secret which ain't generally stored here
const jwtpass='123456789';

const db={};
const tokenToUser={};


function signup(username,req){
    //create the user if ain't exist
    db[username]=req.query.password;
}

function checkUser(req,res,next){
    res.header('Access-Control-Allow-Origin', '*');
    res.header('Access-Control-Allow-Headers', 'Content-Type');
    const username=req.query.username;
    
    if(db[username]===undefined){
        signup(username,req);
        res.json({"msg":'No user found, creating a new one'});
        return 
    }
    else if(db[username]!=undefined && db[username]!==req.query.password){
        res.json({'msg':'Wrong password, User already exist'});
        return
    }
    next();
}
app.post('/auth',checkUser, function(req,res){
    const username=req.query.username;
    const password=req.query.password;
    
    //in db we either have created the user or validated if the user 
    // exist with correct password

    //send the token to user once he or she logged in with correct 
    // credentials

    const token=jwt.sign({username:username},jwtpass);
    console.log(token);
    tokenToUser[token]=username;
    res.status(200).json({
        token
    })
    return

    //this token user will saved in the local storage so every time
    // ain't have to pass (This logic frontend engineer will handle)

})

app.get('/users', checkUser,function(req,res){
    //again if user exist return it
    //else create and return 
    //we alredy have handle creation and check login in checkuser middleware
    const token=req.headers.authorizationtoken;
    const username=req.query.username;
    console.log('token from user',token)
    try{
        const decodedtoken=jwt.verify(token,jwtpass);
        const uname=decodedtoken.username;
        
        if(uname===username){
            res.json({
                username:req.query.username
            })
            return
        }
    }catch{
        res.status(403).json({msg:'wrong token'});
        return
    }
})

const PORT=3000;
app.listen(PORT,function(){
    console.log('server started');
})
```

here now if u hit the server with valid data as follows: First hit to create a new user:

```js
//copy paste directly in browser
http://localhost:3000/users?username=%22saurabh%22&password=%22Pass%22
```

then Try to auth with username and password on auth route: (It is a post request either use postman or fetch is fine)

```js
const d=fetch('http://localhost:3000/auth?username=%22Saurabh%22&password=%22Pass%22',{method:'POST'});
```
Now you'll get a Json web token, You need to store it in local storage if you are implementing the frontend and in further every request you need to have to pass the auth token rather than sending an email and password. not hit the get request again and pass the authorizationtoken.

```js
const d = fetch('http://localhost:3000/users?username=%22Saurabh%22&password=%22Pass%22', {
  method: 'GET',
  headers: {
    'authorizationtoken': 'eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VybmFtZSI6IlwiU2F1cmFiaFwiIiwiaWF0IjoxNzA5Mzg1OTQ3fQ.oJJtCpjSPWf9Egba38c21aZUT4H4P6dPuly1WLipwZ0'
  }
}).then((res)=>console.log(res));
```

and now jwt.verify method will verify it with (with combination of jwt token and Hashed password from database). Here we have not passing hashed password but we have passed the Secret, which usually not store like how we have stored in the code. 

#### Salesforce and Offered abstract Integration.

<hr>
Get to know about authentication && Authorization:
Read :  <a href="https://stackoverflow.com/questions/33702826/oauth-authorization-vs-authentication#:~:text=OAuth%20is%20a%20specification%20for,and%20obtain%20an%20authorization%20grant.">Stack Overflow</a>

**Open Authorization: (Oauth)**
<br>

As with node we have already learn that user don't need to pass the username and password again and again to authenticate himself, the server will provide a token to him so with token user can authenticate, and jwt comprises users information that can be easily decrypted but server db's has the users passwords in hashed format so it can be passed along with the jwt token to authenticate the user every time user opens up the website or ip.
The end goal of O auth is to obtain an access token that can be used to access the protected resources without ever passing the username and passwords to the other systems. The formost example is the mobile phone application, suppose you are using an image edit application , now that application need a access to your image gallary or drive pictures, So you simply allow it but it backed with Oauth. It may first tell to to login with credentials then the server simply provides you an access tokens so now on the image edit application will send an access token to the image server to access the protected resources, in this specific case Images. 

**Authentication and Authorization:**
<br>

if people are trying to access systems then just think of SAML but if systems trying to accesss the system in OAuth. Authentication is the first process for an authorization, a user/system needs to be authenticated first.

Example: Workbench trying to access salesforce data.

**Resource owner :** It may be you who owns the data.

**Client:** client is workbench who try accessing your data.

**Resource Server:** It is salesforce it has protected resources, that can be accessed by client with valid permissions (auth).

**Authorization server:**  this server authenticate the resource owner, issue the access tokens etc.

So oAuth is not created to authorise a person but it meant for a service to authorise another service. We'll look into this in more details in oauth flow.


**Authentication Methods:**
<br>

1. **Form Base Login: (Cookies base auth)**

==> It is simple! first user have to provide username and password. when we hit the sserver with username and password then the server checks or authenticate the user and if authentication is valid then it generate a session using users information and unique session id and set the cookie header with session id value. the server sent cookie has been saved by frontend engineers and after every subsequent request brower send that cookie to the server to identify the users eliminating usage of username and password. and it'll maintain the users sesssion, so everytime you login to github, browser saves that session so every time you don;t have to login.

2. SAML:|| **Security assertion markup language** ||(Single sign-on accross sites):

==> SAML use to achieve the single sign on (SSO). SAML is a markup language with the help we can acheive the sso. at the step 1 the user open a brower and hit an IP of some web application.SAML can be used for both authentication as well authorization, It has 3 entities..The user Agent (Browser), Service Provider (SP) and Identity Provider (IDP). There must be a trust relationship betn SP and IDP, we have to establish the trust relationship (It can be establish by interchanging certificates).First the client who want to access service provider must first authenticate in IDP, Once the user/client is successfully identified/verified the IDP  generate the SAML assertion, the assertion send to the application or SP and since the sp trust idp, the user is now allowed to access service. Now since the user is already authenticated by IDP the user/client now can access other services as well.
<br>

![image](https://github.com/SaurabhMulay999/salesforce-interview-questions/assets/90036775/c1477f15-2dc8-44f7-aaa8-d4fd4617ed77)

<br>

**SAML FLOW:: (in depth)**

The IDP knows about the user/client and their attributes like username, email, firstname, lname or other information that is unique to the user. when IDP generates the saml Assertions then it populate that markup with the user identifier and send it to SP (Service provider) then the SP validate the assertion. But you can't send completely unprotected date or SAML. SO IDP must have sign the assertions before send it to sp, SP will have check the issuer of the assertion. Indirectly sp is validating the idp. (How??? Previously we know that there is trust relation betn them  :  may have been established by sharing certificates.) The SP read the user identifier in his db and try to map it. it may fails if user attribute not found. So the configurations are needed the rules are needed thats tells IDP what the SP needs to check the user in his db. It can be dont using SAML configs. and IDP agrees to match the SAML configs. if both SP and IDP has same SAML configarations then the user can be mapped easily or valiated easily in Service provider. and therby the sp allow the access to the user. The configurations can be done manually in xml metadata files. (IDP.xml and Sp.xml) and idp xml file and sp xml files can contain the settings and certificates (those are metadata files). Npw actually to build the trust IDP sent that xml metadata file to sp and vice versa. 


![image](https://github.com/SaurabhMulay999/salesforce-interview-questions/assets/90036775/4b5e8fe5-c734-42c3-b5bb-36c6e8116852)
<br>

**How user initiate the auth flow:**

**IDP initiated flow:** 

in this flow user starts by accessing IDP. User once authenticated first by idp then after authentication user can request the service.  IDP generates the SAMl assertion and share it with the user, user after share the saml assetion with the Service provider with post request and then (before that the sp will validate the idp as well as the user) in response the session start.


![image](https://github.com/SaurabhMulay999/salesforce-interview-questions/assets/90036775/52a470a1-64fb-480a-a883-6a8fb6fc99bd)

<br>

 **SP initiated flow:**

User start reaching out to service provider directly, since the user is not authenticated the sp redirect the user to the IDP using request for authentication message.once the user is validated the idp generate the SAML assertions and pass it to SP via user agent or user or client and then SP again identify or match the user or identifier passed in the assertion if the user is available or successfully mapped in SP then the session starts. 
 <br>

 **SAML Bindings:** (it shows how the messages and assetions send between sp to idp)

 1. SOAP binding
 2. Reverse SOAP
 3. HTTP redirect binding::: ==> this can be use for "request for authentication messages". send from sp to IDP in the SP initiated flow.
 4. http post binding ::==> this can be user to trasnfer the assertion and request message
 5. http artifact binding
 6. saml url binding

**Http Artifact Binding:** (it needs more clarification)

Using SAMLn artifact, the IDP generates two messages 1.SAML artifact 2.SAML assertion the artifact is very small message contain an unique identifier. the artifact was send via a Post request via client or user agent. Or in simple words User/Client has that artifact which generated by IDP and the user hit post Request to the Service provider with the SAML artifact. Now once artifact received at the service provider the SP send and ACK message ""Artifact Resolved"" to the IDP and now as a response the idp share the SAML assertion. or the "artifact resolve" message has sent just to retrieve the SAML assertion. 

**SAML Assertions Contains:**

1. Name / user indentifier
2. Method of authentication
3. Attributes (More details about the user)
4. Conditions (valid time for assertion , intended audience  to avoid attacks)
5. Issuer ID (So SP will identify who generated this assertions) 

Implement it so you'll get to know more on it!!!!

There are some limitations to the SAML so Oauth came into the picture.

###OAuth 2.0 & OpenId:: 
<br>
Oauth often associated with authentication or sometime authorization, but the thing to remember actually Oauth called Open authorization and why to associate it with authentication??? because authentication is the first step!!!!! But it really not for authentication as well for Authorization but it actually design to get delegated Authority. For example you have 2 apps. app1 and app2 (which might be integrated) and you want to share data in between app1 and app2. Your data is protected so you have to provide credentials to app2 into app1. app1 now has access to all your data and you wont able to decide which data required to share. and providing username and passwords to the apps is not a good idea because they are 3rd party systems.
now with the Oauth when you tell app1 that you want to integrate with app2, it trigger Oauth flow. You as user logged in app2 and within app2 approve integration so app1 never see your credential and as a user you are allow to manage the access the app2 is requesting. you can revoke the access by removing the access token. and you can change password without breaking integrations. 

**Parties**:
0.Resource owner

1.client

2.Authorization server

3.Resource Server  (Authorization server and resource server can be shared or completely saparate)







### Git / Version Control:
<hr>

Don't expect these questions in Interviews but learning perspective or to do open source contribution learning git is important.

To Initialise the git repo:
```js
git init 
```
git status is used to check file status:
```js
git status

No commits yet
untracked files:
    file1
    file2

use git add to track the changes
```
When you activate the version control of your project, your files may  lie in 3 different areas:
1. Working/local area
2. Stagged Area
3. Repository Area

**Working Area:**

The working area is where your file locally resides. Version control is used to track different versions of your code/files. There can be a bunch of files that were not handled by git. or changes in those files
were not yet tracked by git yet. The file which is in a working area is considered to be not in the staging area. and as seen above "Untracked files" return by git status so we can conclude that the
file1 and file2 are in the working area because those files ain't tracked by git. To check files in working area hit git status.

But Git won't be goin to track every single file, you need to mention which file needs to be tracked.

**Staging Area:**
Files in the staging area will be going to tell you what are the Files which are going to be part of your next version. This stagged area is place where git knows what changes will be done from files last version to the next version. So git suggests using git add to add files to stage area. So whichever version of a file you create, you going to add the files using git add.

Try:
````js
git add README.md

````
and hit git status again:
it'll show up a message,

```js
Changes to be committed:
     Use git rm --cached <file> to unstaged (So in my understanding, this cmd will put file again from staging area to working)
     new File: README.md
Untracked files:
     file1
     file2
```

So here we have added README.md from the working area to the staged area by mentioning **git add README.md**.

```
to remove files from the staging area. (So files will again being untracked)

git rm --cached README.md
```

**Repository Area:**
This area contains the details of all your previous registered versions. And for the files in this area, the git knows their history.
To move files from the staging area to the Repository area, it creates a version. and it is being called "Commit"

**Commit is a particular version of the project**, It captures a snapshot of the project's staged changes and creates a version of it.

```
git commit
```
So the overall Flow is to add files from working area to the staging area by using 
```
git add README.md
```

Then to push or to create a new version out of it. you have to commit the changes
```
git commit -m "This is the commit"
```
So now the files that are in the staging area  will be going to be part of that particular version. Remember "Will going to be part of version".

To check all commits:(Listing down all commits of repo)
```
git log

Press q if you want to get out of commit logs.

```

Now suppose you have added the file in the stagged area and have committed the file. If you have added the changes in that particular file. The file is already tracked, so the file won't be going to be untracked.

Example:
1. Git status:

   <img width="293" alt="image" src="https://github.com/SaurabhMulay999/SF_InterviewQuestions/assets/90036775/c681fa70-ddf9-4878-970b-d7ccf99a4028">

3. git add Readme. md and hit git status
   
   <img width="308" alt="image" src="https://github.com/SaurabhMulay999/SF_InterviewQuestions/assets/90036775/f6ae1461-e963-4ba7-9281-fc30da32e0a0">

5. git commit -m "First commit"
   
   <img width="257" alt="image" src="https://github.com/SaurabhMulay999/SF_InterviewQuestions/assets/90036775/2ec326fd-e681-40a0-8b5d-fff77f1ff8e3">

7. Now changes made to that file, the file is already getting tracked.
   
    <img width="296" alt="image" src="https://github.com/SaurabhMulay999/SF_InterviewQuestions/assets/90036775/98059b83-7d4f-4cf4-8202-f9f8bfabfac6">

    But the new File which we create will be untracked:
   
    <img width="299" alt="image" src="https://github.com/SaurabhMulay999/SF_InterviewQuestions/assets/90036775/18f08369-bcf0-417e-8220-a11304402db7">

    If you hit git restore Readme. md. (it will discard the local changes or changes in your working area: changes are reverted and file Readme.md is aint mofied anymore)

    <img width="337" alt="image" src="https://github.com/SaurabhMulay999/SF_InterviewQuestions/assets/90036775/48b56fd3-7b67-48a5-99a1-30779cb70ab3">

IMP:

So git restore removes all file changes from the staging area to be committed. So instead of deleting lines one by one, you can use git restore. Suppose you have a file that is already committed and in the same file if 
you have added a piece of code. So that piece of code will be in your working area. so if you perform git restore then it'll remove the piece of code you have added to the existing file. But if you added that file to the staging area by performing git add Readme. md with that piece of code you have added. So those changes or pieces of code will be now in the staging area, so if you do git restore <file> when you're file is in the staging area the command won't work.

So you need to perform:
```
git  restore --staged <filename>
```
This command will pick staged change and put it into the working area and then you can revert it using git restore.

Example:

First I have added the file to the staged Area by adding a few more changes:

<img width="235" alt="image" src="https://github.com/SaurabhMulay999/SF_InterviewQuestions/assets/90036775/b90c168f-d778-44fb-bf8d-b9b57d9d5325">


Now If you hit git restore Readme. md. The command won't be going to delete changes as the file is in the staging area not in the working area. Now you need to perform : git restore --staged <filename> So your previous changes which you have made or file moved back to the working area.

<img width="362" alt="image" src="https://github.com/SaurabhMulay999/SF_InterviewQuestions/assets/90036775/c8226491-bf02-4320-91dd-1f12a9dcd5b4">

Now you can do "git restore <fileName>" so your recent changes are getting removed.


NOTE: First Time you'll find it critical to understand but start doing small open contributions so it will be very helpful to understand it more.

Then the main question is why we use git rm::
Git rm won't work if your file has changes in the staging area and in the working area. or file itself in the staging area and working area:

```
command:
git rm --cached <filename>
```

Let's try:
Currently, I have an untracked file.

<img width="272" alt="image" src="https://github.com/SaurabhMulay999/SF_InterviewQuestions/assets/90036775/63b8fc7c-dbc0-4ca3-8c74-da8d0c6d8b2e">

Now if I modify that file and add it to the staging area by doing git add.

<img width="230" alt="image" src="https://github.com/SaurabhMulay999/SF_InterviewQuestions/assets/90036775/48475f9b-5d66-478c-a1f7-a5bd97a7b334">


After adding it to the staging area now I locally changed/modified the file. Now my new changes are in the working area.

<img width="295" alt="image" src="https://github.com/SaurabhMulay999/SF_InterviewQuestions/assets/90036775/5aef293a-e372-4179-9ce3-ff2170446695">

Now if I hit git restore, it will discard the changes that are in working area. But if I do git rm <file> It won't allow me if my changes are in the staging area.

<img width="317" alt="image" src="https://github.com/SaurabhMulay999/SF_InterviewQuestions/assets/90036775/4423bfed-3e67-4c83-a8af-1f8ef9b92362">

So we first need to remove that file from the staging area, to perform git rm. so we can use the following command.
```
git restore --staged <filename>

```
Now if you perform git rm --cached <filename>, It'll remove the whole file from the staging area and the file is now untracked. **So if you want your file to be back in an untracked area then only use git rm.**

After performing git rm: file is removed from the staging area and is now untracked.

<img width="265" alt="image" src="https://github.com/SaurabhMulay999/SF_InterviewQuestions/assets/90036775/1b00e9a7-6b1f-4f25-a2d4-5e5c3d7617cf">

**If we just want the changes to be moved to the working area and staging area then use git restore. git restore only manages the last changes**



**GIT DIFF::**

If you want to check the diff between 2 commits:
```
get the commit Ids (To get Commit Id, hit git log)

git diff <firstCommitId> <secondCommitId>

```

Now we have learned about git, what about GitHub??? Publically we need the visibility of the code, we have a local repo that can be shared over the local network but if we need it to be shared publically then we need GitHub repos. How to connect the GitHub repo and your local repo? let's check how!¡¡

```
git remote add origin __URL__
```

**What is git remote??**

Git remote helps us to connect or link  the local repository to the GitHub repository. So with git remote, we can able to access both of the repos. So from local we can push the changes to GitHub or from GitHub we can pull or download the changes to the local repository.

```
git remote

git remote add
```

With git remote add, you can actually establish or create a new repo connection. Then you have to name that connection, generally, ppl write "origin" like git remote add origin, but that name can be anything. it just a name of connection. and after that, you have to mention a URL to establish a connection. 

```
git remote add origin(this origin can named as anything)  __URL__
(Now if you hit **git remote** after this it'll return you "Origin" as the new connection named origin added)

you can also delete established connections by doing:

git remote rm 
```

to push the code to the GitHub repo use:

```
git push origin<remoteName> master

```
to pull the code from the Github repo:

```
git pull origin<RemoteName> master

```
But to pull the code your staging area must be clean. So either you have to stash the changes or you have to commit them.
When someone is modifying the code and you're also doing the same and try to pull the code merge conflict occurs and you have to manually resolve it.


### TypeScript (Fully Optional: LWC initially did not support TS, Officially they are working on it)
<a href="https://github.com/salesforce/lwc/issues/3699#issuecomment-1723278031">TypeScript Support -LWC</a>
<hr>

**So either way, write your LWC code in TypeScript then transpile it down to JS (Locally).**
1. Typescript is a superset of Js or a Javascript with type checking.
2. Most of the open source projects use typescript over javascript. because at compile time only we get to know the errors.
3. Js is kind of interpreted language so At runtime we got to know about the errors so to avoid and have a type safety most of projects (large codebases) use typescript.

Let's play with TS:

0. Install Ts npm i -g typescript
1. First open a VS code, Type npm init -y to initialize in a terminal.
2. Then to create the TS config file: write, npx tsc --init. (it will create tsconfig file)
3. create a file: touch index.ts
4. write a code now. 

```js
index.ts

let num: number=10;
console.log(num);
num="saurabh"

```

Usually, browsers or Node don't know Typescript. So Typescript always compiles down to JavaScript. So browser and Node env only understood Javascript. and TsConfig file contains the settings that we can enable or disable and as required it determines how we can run or compile TypeScript. 

TypeScript Compiler: tsc is the compiler used to convert Typescript to JavaScript. 

```js
node index.ts
```

It'll throw error: So first run to compile

```js
tsc -b

//first it'll throw the error as the above code:
//type of num is Number and we are trying to assign a string to it:

So modify the code as:

let num: number=10;
console.log(num);

now run
tsc -b

```
that typescript compiler: tsc -b will search for the config file. If no config file is found it'll not goin to transpile the code. 

Now it will create another file named: (If no compile error is found), Now you can run that Javascript file:
```
node index.js
```

To deal with Functions:

```js
Types: String, boolean, number, null, undefined


function fn(string:name):<Return Type> number{
   console.log('Hii',name);
}
let name:string='saurabh';

fn(name);

//Now the type must not be implicitly any so Ts says tell me explicitly if the parameter is any.
so,

function fn(Name){
}

will give you an  Error and now you need to mention the parameter type explicitly.

function (name:any){} or function(name:string){}

```

Code: to add two Numbers:
```ts
let n:number=2;
let m:number=4;

funtion fn(n:number, m:number):number{
    return n+m;
}

let res:number=fn(n,m);
console.log(res);

```

But functions can infer the types, So it will be optional to write the return type explicitly. 

Example:

````js
function fun(fn){
    setTimeOut(()=>{fn();},1000);
}

````

So here what will be the type of function that we have passed as a parameter?
it's a function definition.
like!¡¡

```js
function fun(fn:()=>void){
    setTimeout(()=>fn(),1000);
}

function fn(){
    console.log('hii');
}

fun(fn);
```
In the above example, the function fn passed as a parameter and type we have mentioned as ()=>fn infer that the function accepts nothing and returns the void.
same we can have for the sum function: which accepts 2 param and returns a number.
for example:

```js
function fun(sum:(a:number, b:number )=>number){
    setTimeout(()=>sum(2,100),1000);
}

function sum(a:number , b:number){
    console.log(a+b);
    return a+b;
}

fun(sum);
```

TS Config File::

1. the configs and settings:
2. Target: option in the file specifies the ecmaScript target which the ts compiler will compile the ts code down.

**More on Target:** 
As we know Js is not forward compatible, like you cannot run the code that you have written now in latest version in the old browsers. Because those browser versions ain't support that.
So at some point, we may have a requirement where the browser is quite old and needs to run the old js, So ts target can define the ECMAScript target like telling the compiler "Hey compiler this is what I have written in a typescript file now please transpile it down with a version of ECMA2016 or else." So anyway you're writing the code in the new/latest version of JS but the babel/ts internally traspile it down.

```js
tsconfig.js
{
"target": "es5",
}

```

3.rootDir and outDir: So the transpiler generates whole new js file which can be run in the browsers and node env.So specifying where the out .ts file resides can be done with rootDir and the generated .js file can be determined by outDir.

**More on rootDir and outDir:**
Currently both files, .ts file, and .js file (which has been generated by tsc -b) resides in the same folder. It can be annoying if we have n number of components. So to specify where to store .ts file we use rootDir. and usually we create folder **Build** or **dist** to store the files which are generated by transpiler. (.js files)

```js
{

"target":"es5"
"rootDir":"./saurabh/src/",
"outDir":"./saurabh/build/",

}
```

**Note:(IMP)**
**Never push these .js files to git.** You can add the folder build or dist to .gitignore. In higher environments, you have the file: package.json or lock or configs that will handle the transpilation.
So never commit these files to Git Hub.

4. NoImplicitAny:: if true then it enables Error Reporting for the expressions and declarations with an implied any type. it controls the strictness.
5. remove comments: true remove the comments from the transpile js file.
6. ZOD is for runtime checks.


**Interfaces:**
To assign the types to an Object then we use interfaces.

Without an interface the program could look like this::

```js
const product={
name:'mobile',
cost:12441,
weight:123
}

//either in ts you can do

function fun(product:{name:string,cost:number,weight:number}):void | string{
    if(product.cost>100){
        return 'Cost is too high'
    }
}

console.log(fun(product));
```

Now here every time in the functions like fun we have to mention the definition of the product object: to overcome this issue or to impose reusability we use interfaces:

Using interface Product we can mention the defination of product object is like Product interface:
```js
interface Product{
    name: string,
    cost: number,
    weight: number
}

const product={
name:'mobile',
cost:12441,
weight:123
}

//either in ts you can do

//look here the function has either void or string as a Return type: These can be implemented with the | operator.

function fun(product:Product):void | string{
    if(product.cost>100){
        //fn returning string
        return 'Cost is too high'
    }
    //fn returning void/undefined
}

console.log(fun(product));
```
Optional parameters in an interface can be defined with the '?' operator:
```js
//here edible is the optional prop.

interface Product{
    name: string,
    cost: number,
    weight: number
    edible?: boolean
}
```

**Implementing the Interface:**

Interfaces can be implemented by a class.

Example:
```js
interface Product{
    name: string,
    cost: number,
    weight: number
    edible?: boolean
    //we only need a declaration of the function | method over here
    //also we can't use function keyword: it may assume it would be a function
    buy(id:number):string;
}

class electronics implements Product{
    name: string
    cost: number
    weight: number

    buy(id:number){
        return `${id} product bought successful`;
    }
    constructor(name: string,cost: number,weight: number){
        this.name=name;
        this.cost=cost;
        this.weight=weight;
    }
}

let e=new electronics('Mobile',1221,21);
console.log(e.buy(e.cost)); 
```

So interface can only have a defination of methods and properties but not the method implementation. Interfaces ain't used that much.
So types come into the picture. but interfaces can be implemented with classes but types can't be.

**Types**::

Similar to Interfaces types can be used to aggregate the data together.

To define a type:(kind of same as interface)But types can't be used to implement by classes.
```js
type product={
name: string,
cost: number,
}
```

 but types can be used to do some other sort of stuff.

 UNION:

```js
//Type can be either of 2 types either string or number

type Snumber= string | number;

function sum(strNum: Snumber){
    console.log(strNum);
}

//so now it can accept both
sum('12323');
sum(1232);
```

INTERSECTION:

if you want to create a type that has every property of multiple types/interfaces!¡¡¡

example:

```js
type product={
    name: string,
    cost: number
}
type seller={
    Sellername: string,
    id: number
}
//seller or product can be interfaces also.

type FinalProd=product & seller;

const p: FinalProd={
    name:'Mobile',
    cost:12,
    Sellername:'asthetic',
    id:12321
}

console.log(p);
```
**Interview question:**
What is diff between the type and interfaces??? In above article we have covered those points!¡¡

**Arrays and Enums in Typescript::**

arrays in js can hold different data types. So to define a type we use <type>[];
Only type can do this::
````js
type NumberArray=number[];
function getElementatI(arr:NumberArray){}
OR
function getElementatI(arr:number[]){}
````

**Problem**: Create an array of Students: with Students is an object with properties name and marks and filter down the students with marks less than 50.
Solution:

```js
interface Students{
    name: string,
    mark: number,
}

//create the students with names and marks
let arr:Students[]=[];

for(let i=0;i<10;i++){
    const student={
        name:'s'+i,
        mark:Math.floor(Math.random() * 90 + 10),
    }    
    arr.push(student);
}

//create a filter function to filter down student marks less than 50

function filter(arr:Students[]):Students[]{
    const array:Students[]=[];

    for(let stu of arr){
        if(stu.mark<50){
            array.push(stu);
        }
    }
    return array;
}

console.log(filter(arr));
```
An array with multiple data types can called a tuple in TS, It ain't the same implementation as Python.
```js
function fun():[number,string,boolean]{
    return [1, 'Stre', true];
}
```

**Enums:** (JS don't have enums)

Enums in ts allows you to define the set of named constants.
a way to present a set of constant values. which can be represented as numbers or strings.

why do we need Enum:

```js
//suppose a function does move you're Chessboard queen

function moveDirection(direction){
    //logic based on direction
}

//now here I can pass anything in function irrespective of the valid values.
//example: and now 'Saurabh'  is not a valid argument, so to enforce that we can use an enum.

moveDirection('saurabh');
```

another best way to resolve this issue is with types:

```js
type direction=" UP" | "DOWN" | "Diagonal";
```
so now with types we can enforce these only set of values to the function parameter.

How to use Enums:

```js
enum Dir{
      Up,
      Diagonal,
      Left,
      Right,
      down
}

//I have provided the enum values above:

function moveDirection(direction:Dir){
    if(direction=== Dir.Up){ //pass }
}

//so it can be used if you have a limited set of values, you can define it in enum.

```

**Generics:**
Suppose you don't know the array data type and you have to return values from that array (assume array.length>0 ) so you have to return array[0] but it can be either any data type!¡¡

this issue can be solve with below approch: But what is the issue with that approch???

```js
type arrayInput= string | number | boolean;

function print(array:arrayInput[]):arrayInput{
    return array[0];
}
const v=print(['sarah','srbh']);
console.log(v);  //Will print sarah

```

For now the return type is string, Now try using string related methods on it.
```js
type arrayInput= string | number | boolean;

function print(array:arrayInput[]):arrayInput{
    return array[0];
}
const v=print(['sarah','srbh']);
console.log(v.charAt(0));  //COMPILE TIME ERROR: That the charAt can't be used on type Number.

```
So rather than the tuple you can have type as either string[] or the number[] so type arrayInput makes no sense.

```js
function print(array: number[] | string[] ):arrayInput{
    return array[0];
}
```
But better way to solve is through generic programms.
T => can infer the type correctly (you'll understand if familier with Generics in java)

**So Generics can work with any data types and still provide the compile time type safety!¡¡**

Example:

```js
//args can be auth token or password or otp

function authenticate<T>(arg:T):T{
    return arg;
}

let user1=authenticate<string>('Pass@123');
let user2=authenticate<number>(1233);

console.log(user1.charAt(0),user2)  //now it will work!¡¡
```



