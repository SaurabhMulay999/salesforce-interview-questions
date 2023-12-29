### Salesforce Security Model
<hr>

**Explain Salesforce Security Model: With Examples and Theory.**

Who sees what in SF....!!!!!!!!!

 The bare use of the security model is to avoid unauthorised access to the application or to our data. It is divided into following types we can say.
  
  1.UserName password (need username and pass to login)
  
  2.Login IP based security (you must be within the ip range to access data)
  
  3.Login hrs (u can set the hrs: example user/admin can only login within hrs)

  **Types of securities:**
  
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

  **Explanation of Object level Security:**

  It can be implemented using profiles and permission sets. when u have multiple users and in db's table they need different diff access in term of CRED (crete, 
  read,edit, delete). so for the users you can have unique profile, profile is set of access priviledge and settings given to the user. Every user has profile
  associated with it. one user one profile and basic accessability of a user is dependant on the profile. one profile can be associated with many users. (one to 
  many rel). System admin is the profile with all accesses. User: license is not consumed on creation of profile but consumed on creation of users associated 
  with that profile. free dev org has only 5mb space. business's need more so option is to buy the license to have more users and space. Most of MNC's used 
  unlimited version. per users may be costs 300$. feature licens also available with salesforce. Features can be accessable but need to buy it, suppose you need
  sales cloud, service cloud, marketing cloud. those features are need to buy.   
  
  
  
  
