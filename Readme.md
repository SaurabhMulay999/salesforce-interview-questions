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
  ```
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
    
```` 
SELECT LeadSource, COUNT(Name) cnt FROM Lead GROUP BY ROLLUP(LeadSource)

Returned Aggregated results include an extra row with a null value for LeadSource that gives a grand total for all the groupings. Since there is only one 
rollup field, there are no other subtotals.

`````
5.Filtering soql queries With SECURITY_ENFORCED (Must asked Interview Question)
```
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
Prerequisite: Javascript (If ain't have learnt yet,Fuck off!!)

Decorators:

**Public properties**:: 
The public properties are decorated using @api and they are also reactive, when the value of property changes it results redering component and invoking rendercallback() hook. Though they are reactive but Js playes very important role here,

consider below scenarios::
```
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
```
@track const obj={
 @track prop:{
  X:10;
 }
}
```

Note while using @track :; dding a new property to obj or a change doesn’t trigger a rerendering.

example:

it's just js habits to declare he variables with var and let but in classes you don't have to do that.
````
@track const obj={val1:"saurabh"}
obj.val1="sarah" //will trigger the rendering

but:
obj.val2="saurabh" //won't be triggering the rendering

````

so again they fucked up over here, Likw kindoff forgot to handle the edge cases so they have suggested as per the docs:
[[To rerender your component when adding a new property, assign the object to a new object with both values.]]

like:
```
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
```
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

````
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
````
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
<hr>
