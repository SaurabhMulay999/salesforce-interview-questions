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
  setting)","public read only" ,"private" ] }. private is most restrictive one. To open up the access we have sharing rules, if owd has locked up the records


 
  
  
  
  
  
  
  
