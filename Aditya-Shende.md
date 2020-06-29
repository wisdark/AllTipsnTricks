1. 
Found simple oauth stealing

Endpoint:
auth?response_type=code&redirect_uri=

Explanation ->

They were not validating the domain is evil or good

redirect_uri parameter in OAUTH flow, its possible to redirect authenticated users to arbitrary domains with their oauth credentials from which its possible to takeover their account.

So redirect_uri accept any url like attcker.com

How he found endpoint ->

# 1.Was searching some endpoints on github with following
"http://site.com" url=
"http://site.com" redirect=
"http://site.com" uri=

# 2. Found login function endpoint and accessible URL

# 3. The same function was their all 7 subs. All affected

2.

Bug name: Unauthorized access to API EP via jql queries

Basically jql is low based but I escalated to good severity 

JQL -> Java Persistence Query Language

Basically normal query responding in normal data but error based disclosing endpoints where path was unauthorized for viewing Json endpoints

Resource -> https://community.atlassian.com/t5/Jira-Questions/JQL-input-sanitization/qaq-p/967393


3. 
Testing for confluence(Older version)
Found CVE:-2018-20824

Created dork: inurl:"/plugins/servlet/Wallboard/"

EP:/?dashboardId=10102&dashboardId=10103&cyclePeriod=(function(){alert(document.cookie);return%2030000;})()&transitionFx=fadeZoom&random=false

Resource ->
https://www.cybersecurity-help.cz/vdb/SB2019050605

Some programs still using old versions: Hit and try

/plugins/servlet/Wallboard/?dashboardId=10000&dashboardId=10000&cyclePeriod=alert(document.domain)


4.
I was testing for ATO via reset function . Tried all method but no success. My friend 
@Tabnexa
  gave me tip to add  double Host in request while requesting password

Host: http://site.com
Host: http://evilsite.com

Boom it worked 

Resource ->
https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/17-Testing_for_Host_Header_Injection

Read this and there are multiple bypasses on h1 .

What did you achieved by add double host? -> Password Reset Poisoning

What are the other things we can try??

I tried this method, permuting email and token, adding extra email, X-Forwarded Headers, X-Host header,  HPP... What else can we do???

Host,   X F H ,Vice versa of them....

ATO -> Account Takeover
XFH -> X-Forwarded-For

Resource ->
https://www.youtube.com/watch?v=HgCLbeUegfI&feature=youtu.be



5.

Accidental finding (4 days ago)
In site article they mentioned "If you have any queries, You can raise ticket on http://site.atlassian.net"

1. When I clicked on link : Doesn't exist
2. So I created the domain as they mentioned (http://site.atlassian.net)
3. But no information....
4. So I thought to check domain again.
5. In recent tickets I found 7 raised queries . Some of them mentioned their personal information

Quick report to company

Reference -> https://medium.com/@intideceukelaire/hundreds-of-internal-servicedesks-exposed-due-to-covid-19-ecd0baec87bd

Summary :-> 
He saw an ad and visited that link. But it's empty, so he created that subdomain site(by using rouge cname). Few customers visited his site and submitted personal info for tickets. He submitted that to company as it's a privacy policy violation



6. 
Able to download anyone's report
Function: You can create an own report and after that you can download it via csv or txt file

1. Go to report section 
2. Download option-> Click on txt
3. Capture request > Do intercept > Response to this request
4. Username & Filename disclosed
5. Format :- aditya-1.txt
6. Changed aditya to other username (eg: jonas-1.txt)
7. It was downloading jonas 1st report


7.
Got SSH access
❣️❣️❣️❣️
Tip: Github Dork
"site . com" ssh language:yaml

Got config.yaml

you can also check yaml, sql, txt, ruby

Resource -> 
Exploiting the SSH CRC32 Compensation Attack Detector ... - Penetration Testing
https://github.com/techgaun/github-dorks

NOTE: If got ssh port open and access denied -> Check for creds on github, If you get it ,you are lucky


8.
Upload RCE 
1. Checking functions manually
2. Found document section where we can upload documents of daily report.
2. Tried simple .php, php2,phps,php4 no success
3. I changed content & file type in PUT request with .phps and got success
4. /any.phps?cmd=cat+/etc/passwd

One more tip: Use intruder for HTTP verb checking - Payloads - Add from list - HTTP verbs

Another part I found document upload IDOR on same function❣️


9.
In last 3 days found 2 Remote Code Execution

Tip 1: Always check document upload section

2. If your request is going to admin try to escalate using burp-collaborator.

These are tips only remaining you've to Google Heart exclamation ❣️

Resource -> https://medium.com/@satboy.fb/art-of-unrestricted-file-upload-exploitation-92ed28796d0

For 2: meaning OOB getting triggered on your collaborator / bind server by passing the output from RCE as DNS-request? What kind of document you talking bout? XXE with docx/SVG/xlxs?

just 2 words I'll tell you

HTTP
wget


10.

P1 
Postgresql conf data disclosure
1. Site with bulky functions
2. Started long fuzzing via burp
3. Found some juicy points but no idea what to do next
4. Started URL fetching and dirsearch
5. Multiple dir found
6. Conf file disclosed critical information 


Its simple directory disclosing configuration files. The main keyword was /source/

What do the terms "bulky functions" and "long fuzzing via burp" mean?

site having multiple processes 
You can check intruder payloads section for long dir payloads



11.
Unauthorized access to event mgt system:
Function- You can create public or private invents

1. site.com/xyz/username?view=current_events
2. Change username and forward request
3. Able to just view title, date created and event owner name
4. Escalated to access via manual headers
5. Used X-Rewrite-URL: /current_events
6. Forward request . Now able to see full event data
7. For performing every step I need to add X-Rewrite-URL: /action_here 

Tip: Always add headers to bypass single based verification on sensitive action.
P2 marked as P1


12.
Title -> Some keywords you must search and focus while hunting:

https://twitter.com/ADITYASHENDE17/status/1253624528871272450

13.
the issue is http parameter polution
Account takeover 
Function: You can reset link to email or phone
1. Captured request of reset link via phone number ("number:xxxx")
2.  Added same parameter with different number 
3. Do intercept> Response t this request = Reset link sent on 1234, Reset link sent on 4567
4. Got link on both numbers 
5. Both link worked

{"number":"1234","number":"5678"}

I tried on email function also but no success

EXTRA ->

Does this make sense? (array)

{ "email" : [ 
        "userone@gmail.com", 
        "attacker@gmail.com" 
] }

and

Got adviced by a friend
email=victim@email.com&email=attacker@email.com

email=victim@email.com,attacker@email.com

email[0]=victim@email.com&email[1]=attacker@email.com


Reference -> https://hackerone.com/reports/322985

14.

Open access to Internal management console.

1. site having pretty good scope to test.
2. Started with google dorks , Basically index dir
3. site with some directories but not able to access
4. Dirsearch started with json,php, aspx
5. Php found but no success 
6. On manual observation found basic console button under that php files > Click > Boooom
6. Too much sensitive data

Dorks example ->

1. site:http://site.com ext:xml | ext:conf | ext:cnf | ext:reg | ext:inf | ext:rdp | ext:cfg | ext:txt | ext:ora | ext:ini

2. site:http://site.com intitle:index.of


15.
Account takeover
1. 1 account logged in 2 browsers
2. Tried signup with same account but showing email exist and redirect to signup page
3. In Firefox captured request of sign up submit >Do intercept > Response > Email exists
4. Response changed to E-mail available >302 found /dashboard. Account created
5. Change profile data
6. Refresh in chrome and data changed

After successful registration it was redirecting to user account dashboard, That's why I used 302 Found /dashboard

Its 2FA bypass but you'll get idea how tampering helps to bypass

Reference -> https://www.youtube.com/watch?v=mFeRAFWjEns&feature=youtu.be
