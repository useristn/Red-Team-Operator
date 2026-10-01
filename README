# 1. Bức tranh tổng thể của một Red Team operation

Đừng hiểu Red Team đơn giản là:

> scan → exploit → lấy shell.

Một campaign thực tế có thể nhìn theo chuỗi:

```text
Target
   │
   ▼
External Recon
   │
   ▼
Attack Surface Mapping
   │
   ▼
Initial Access
   │
   ▼
Payload Execution
   │
   ▼
Command & Control
   │
   ▼
Host Recon
   │
   ├── User / Privilege
   ├── Security Controls
   ├── Credentials
   └── Domain Context
   │
   ▼
Persistence
   │
   ▼
Internal Network Recon
   │
   ▼
Active Directory Recon
   │
   ▼
Privilege Escalation / Lateral Movement
   │
   ▼
Objective
```

Trong syllabus hiện tại, bạn đã được học khá rõ phần **từ Recon đến Internal Recon**, nhưng hai mắt xích lớn là **Privilege Escalation** và **Lateral Movement** chưa xuất hiện thành module độc lập. Đây là điểm quan trọng cần nhận ra khi review.

Một operator tốt phải luôn trả lời được năm câu:

| Câu hỏi | Ý nghĩa |
|---|---|
| Tôi đang ở đâu? | External, workstation, server, domain member...? |
| Tôi đang chạy dưới identity nào? | Local user, domain user, admin, SYSTEM...? |
| Tôi nhìn thấy gì? | Host, service, user, share, domain, subnet... |
| Tôi tạo ra telemetry gì? | Process, network, authentication, registry, service... |
| Thông tin vừa có giúp tôi quyết định bước nào tiếp theo? | Recon tiếp, persistence, escalation, lateral movement...? |

Đây mới là **operator mindset**.

---

# 2. External Reconnaissance

Training bắt đầu bằng việc xác định tổ chức, website, vị trí hạ tầng, subdomain và các dịch vụ exposed ra Internet; sau đó chia thành passive và active reconnaissance. Pasted text

## 2.1 Recon thực sự nhằm tìm gì?

Không chỉ là “tìm IP”.

Bạn đang xây dựng **attack surface map**:

```text
Organization
 ├── Domains
 │    ├── example.com
 │    ├── careers.example.com
 │    ├── vpn.example.com
 │    └── mail.example.com
 │
 ├── IP ranges
 │
 ├── Public services
 │    ├── HTTP/HTTPS
 │    ├── VPN
 │    ├── Mail
 │    └── Remote management
 │
 ├── Cloud assets
 │
 └── People
      ├── IT
      ├── HR
      ├── Developers
      └── Administrators
```

Recon tốt phải chuyển dữ liệu thô thành **candidate attack paths**.

Ví dụ về tư duy:

```text
careers.example.com
        ↓
Recruitment application
        ↓
Có upload CV
        ↓
Có interaction với HR
        ↓
Có thể tồn tại trust relationship giữa external recruitment system
và người dùng nội bộ
```

Đó chính là logic dẫn sang scenario của tuần Initial Access trong tài liệu.

---

# 3. Passive Recon vs Active Recon

## Passive Recon

Không tương tác trực tiếp đáng kể với target.

Syllabus đưa ra:

- OSINT
- Google Dorking
- Shodan. Pasted text

Điểm cần hiểu là:

```text
Passive Recon
      ↓
Low interaction
      ↓
Lower observable footprint
      ↓
Phù hợp giai đoạn xây attack surface ban đầu
```

### OSINT

Có thể thu thập:

```text
Organization
Employees
Technologies
Domains
Public documents
Email formats
Cloud services
Source-code exposure
Recruitment information
```

Đừng chỉ hỏi:

> “Tìm được gì?”

Hãy hỏi:

> “Thông tin này có mở ra attack path nào không?”

Ví dụ:

```text
Job posting
   ↓
"Experience with Microsoft 365 + SharePoint"
   ↓
Suy luận infrastructure sử dụng Microsoft ecosystem
```

Nhưng phải phân biệt:

```text
Fact:
Job description nhắc đến SharePoint.

Inference:
Tổ chức có khả năng sử dụng SharePoint.

Không được biến inference thành fact.
```

---

# 4. Google Dorking

Mục tiêu của dorking không phải “hack Google”.

Nó là khai thác khả năng indexing để tìm:

```text
Public documents
Backup files
Configuration remnants
Directory listings
Subdomains
Login portals
Error pages
Technology information
```

Điều quan trọng nhất trong review là hiểu **information exposure**.

Một file công khai đôi khi tiết lộ:

```text
username convention
hostname
internal domain
software version
department structure
email format
```

Những thông tin nhỏ đó khi ghép lại tạo thành intelligence.

---

# 5. Shodan

Shodan giúp nhìn Internet theo hướng **service-centric** thay vì website-centric.

Một asset có thể xuất hiện dưới dạng:

```text
IP
 ├── Port
 ├── Service
 ├── Banner
 ├── Product
 ├── Version
 └── Certificate metadata
```

Điểm quan trọng:

> Port mở không đồng nghĩa với vulnerability.

Bạn phải phân biệt:

```text
Port open
      ↓
Service identification
      ↓
Version / configuration
      ↓
Exposure analysis
      ↓
Vulnerability hypothesis
```

---

# 6. Active Recon

Syllabus tập trung vào:

- Port scanning
- DNS enumeration
- Banner grabbing. Pasted text

Active recon tạo traffic tới target nên khả năng bị quan sát cao hơn passive recon.

## Port

Port chỉ là **logical communication endpoint**.

Ví dụ conceptually:

```text
Host
 ├── TCP/80   → HTTP?
 ├── TCP/443  → HTTPS?
 ├── TCP/445  → SMB?
 └── TCP/3389 → RDP?
```

Nhưng:

```text
Port number ≠ guaranteed protocol
```

Do đó cần **service identification**.

---

# 7. Banner Grabbing

Banner có thể tiết lộ:

```text
Protocol
Product
Version
Hostname
Implementation
Configuration
```

Ví dụ logic:

```text
Port
 ↓
Banner
 ↓
Product
 ↓
Version
 ↓
Known vulnerabilities?
 ↓
Configuration weakness?
```

Điều quan trọng trong report:

```text
Observation ≠ Exploitability
```

Bạn chỉ được kết luận vulnerability khi đã có bằng chứng phù hợp.

---

# 8. DNS Enumeration

DNS là nguồn reconnaissance cực kỳ giá trị.

Bạn cần hiểu ít nhất các record:

| Record | Ý nghĩa |
|---|---|
| A | hostname → IPv4 |
| AAAA | hostname → IPv6 |
| CNAME | alias |
| MX | mail server |
| NS | authoritative DNS |
| TXT | metadata / verification information |
| PTR | IP → hostname |

DNS enumeration giúp xây:

```text
Domain
   ↓
Subdomain
   ↓
Hostname
   ↓
IP
   ↓
Service
```

Đây chính là **asset graph**.

---

# 9. Initial Access

Syllabus định nghĩa mục tiêu của tuần 2 là đạt foothold ban đầu và hiểu payload cũng như social engineering trong môi trường lab. Pasted text

Một điểm rất quan trọng:

```text
Initial Access ≠ C2
```

Initial Access chỉ là cách đưa attacker-controlled execution vào target.

Sau đó mới có:

```text
Execution
    ↓
Agent
    ↓
C2
```

---

# 10. Payload taxonomy

Bạn cần phân biệt rõ bốn thứ:

```text
Shellcode
Dropper
Stager
Full payload / Agent
```

## Shellcode

Shellcode là đoạn machine code được thiết kế để thực thi trực tiếp trong memory.

Mental model:

```text
Shellcode
   ↓
CPU instructions
   ↓
Memory execution
   ↓
Performs specific behavior
```

Shellcode không đồng nghĩa malware.

Training của bạn thậm chí sử dụng một shellcode vô hại chỉ hiển thị:

> “Hello woo”

để hiểu execution flow. Pasted text

Điều cần hiểu sâu là:

```text
Architecture matters

x86 shellcode → x86 execution context
x64 shellcode → x64 execution context
```

---

# 11. Dropper

Dropper có nhiệm vụ **đưa hoặc cài một payload khác**.

Concept:

```text
Dropper
   ↓
Contains / retrieves payload
   ↓
Writes or prepares payload
   ↓
Executes next stage
```

Dropper không nhất thiết là payload cuối.

---

# 12. Stager

Stager thường là một component nhỏ với nhiệm vụ:

```text
Initial code
   ↓
Establish communication
   ↓
Retrieve larger stage
   ↓
Execute stage
```

Khác biệt tư duy:

| Dropper | Stager |
|---|---|
| Thường triển khai payload | Thường tải/khởi tạo stage sau |
| Có thể ghi file | Thường tối ưu kích thước |
| Deployment oriented | Communication/bootstrap oriented |

---

# 13. Office Macro

Syllabus yêu cầu hiểu VBA Macro và XLM Macro cùng cơ chế và biện pháp phòng vệ. Pasted text

Hai khái niệm:

```text
VBA
Visual Basic for Applications

XLM
Legacy Excel macro language
```

Điểm Red Team cần hiểu không phải thuộc syntax macro, mà hiểu chain:

```text
Document
   ↓
User interaction
   ↓
Macro execution
   ↓
Process creation / script execution
   ↓
Payload
```

Blue Team nhìn chain này thông qua:

```text
Office process
      ↓
Unexpected child process
```

Đây là một telemetry quan trọng.

---

# 14. Script-based Payload

Training nhắc:

- PowerShell
- HTA. Pasted text

Điểm cần hiểu là **execution host**.

Ví dụ:

```text
Script
  ↓
Interpreter
  ↓
Windows APIs / .NET
  ↓
Action
```

Script không tự chạy. Luôn tồn tại một execution engine/interpreter.

Đây là điều rất hay bị bỏ qua khi học Red Team.

---

# 15. Social Engineering và ClickFix

Scenario của syllabus rất hay ở chỗ:

```text
External recruitment site compromised
              ↓
Không có đường kỹ thuật trực tiếp tới internal network
              ↓
Human interaction becomes potential bridge
```

Tài liệu yêu cầu hiểu ClickFix, cách người dùng bị dẫn dụ thực hiện hành động nguy hiểm và cách nhận biết/phòng chống. Pasted text

Điểm bản chất:

```text
Technical trust boundary
không vượt được
        ↓
Human trust boundary
có thể bị khai thác
```

Đó là lý do social engineering tồn tại trong Red Team.

---

# 16. Command & Control

C2 là phần cực kỳ quan trọng trong training. Mục tiêu là thiết lập communication giữa foothold và hệ thống điều khiển. Pasted text

Mental model:

```text
               Operator
                   │
                   ▼
              C2 Client
                   │
                   ▼
              Team Server
                   ▲
                   │
               Listener
                   ▲
                   │
             Agent / Beacon
                   ▲
                   │
                Victim
```

Bạn phải phân biệt các component.

---

# 17. Team Server

Team Server chịu trách nhiệm:

```text
Listener management
Agent communication
Tasking
Session state
Operator collaboration
Logging
```

Không nên gọi mọi thứ là “C2 server”.

Team server chỉ là một component trong architecture.

---

# 18. C2 Client / Admin

Đây là UI mà operator sử dụng.

Syllabus chia Havoc Admin thành các nhóm:

- listener
- agent/session
- command execution
- file management
- system information
- task/log management. Pasted text

Mental model:

```text
Operator action
     ↓
Admin client
     ↓
Teamserver
     ↓
Task queue
     ↓
Agent
     ↓
Execution
     ↓
Result
     ↓
Teamserver
     ↓
Admin client
```

---

# 19. Listener

Listener là server-side component nhận communication từ agent.

Ví dụ conceptually:

```text
HTTP Listener
HTTPS Listener
TCP Listener
SMB-related communication channel
```

Listener thường có:

```text
Address
Port
Protocol
Profile
Authentication/config
```

---

# 20. Agent / Beacon

Syllabus định nghĩa agent/beacon là component chạy trên client và kết nối về Havoc server. Pasted text

Agent thực hiện hai chức năng chính:

```text
Communication
+
Task execution
```

Cycle đơn giản:

```text
Agent
  ↓
Check-in
  ↓
Get task
  ↓
Execute
  ↓
Return result
  ↓
Sleep
  ↓
Repeat
```

---

# 21. Session

Session **không phải bản thân agent**.

Session là representation của agent connection trên operator interface.

Ví dụ:

```text
Agent process exists
        ↓
Agent communicates with C2
        ↓
Teamserver registers agent
        ↓
Operator sees SESSION
```

Nếu connection mất:

```text
Agent có thể vẫn tồn tại
nhưng session trở thành inactive/dead
```

---

# 22. C2 protocols

Training yêu cầu hiểu:

```text
TCP
HTTP
HTTPS
SMB
DNS
...
```

Không cần học chúng chỉ như danh sách.

Hãy hiểu theo trade-off:

| Protocol | Điểm cần nghĩ |
|---|---|
| HTTP | Phổ biến, dễ hòa vào web traffic |
| HTTPS | Có TLS |
| TCP | Communication trực tiếp hơn |
| SMB | Có thể hữu ích trong internal communication |
| DNS | Traffic pattern rất khác HTTP |

Một operator phải nghĩ đồng thời:

```text
Connectivity
Stealth
Reliability
Network controls
Telemetry
```

---

# 23. Direct vs Reverse Connection

Training yêu cầu phân biệt hai dạng này. Pasted text

### Direct

```text
Operator
   │
   ▼
Target
```

Target lắng nghe.

### Reverse

```text
Target
   │
   ▼
Operator-controlled infrastructure
```

Target chủ động tạo outbound connection.

Điểm quan trọng:

> Trong enterprise environment, inbound traffic thường bị hạn chế mạnh hơn outbound traffic.

Do đó reverse communication rất phổ biến trong các simulation.

---

# 24. Weaponization

Tuần 4 chuyển sang DLL Hijacking và signed binaries/LOLBIN trong lab. Pasted text

Điểm lớn nhất cần hiểu:

```text
Exploit software vulnerability
```

không phải con đường duy nhất.

Một attack path cũng có thể đến từ:

```text
Application behavior
+
Unsafe loading mechanism
+
Environment configuration
```

DLL Hijacking là ví dụ điển hình.

---

# 25. DLL là gì?

DLL:

```text
Dynamic Link Library
```

Windows process có thể load code từ DLL thay vì compile tất cả vào executable.

Simplified:

```text
Application.exe
       │
       ├── DLL A
       ├── DLL B
       └── DLL C
```

---

# 26. DLL Search Order

Khi application yêu cầu:

```text
example.dll
```

nhưng không cung cấp absolute path, Windows cần xác định:

> DLL đó nằm ở đâu?

Nó tìm theo các location nhất định.

Nếu attacker-controlled directory xuất hiện ở vị trí phù hợp trong search order:

```text
Application
    ↓
Requests DLL
    ↓
Search paths
    ↓
Wrong / attacker-controlled DLL selected
    ↓
Code loaded into process
```

Đó là nền tảng của DLL Hijacking.

---

# 27. DLL Hijacking vs DLL Side-loading

Hai khái niệm thường bị trộn.

### Hijacking

Lợi dụng search/load behavior để application load DLL không mong muốn.

### Side-loading

Một legitimate binary, thường có trust/signature phù hợp, load một DLL nằm cạnh nó.

Mental model:

```text
Legitimate EXE
       +
Controlled DLL
       ↓
DLL loaded
       ↓
Code executes inside legitimate process
```

Training của bạn tiếp tục dùng kỹ thuật này trong phần persistence. Pasted text

---

# 28. Persistence

Persistence trả lời câu hỏi:

> Nếu user logout hoặc máy reboot, quyền truy cập còn tồn tại không?

Tài liệu yêu cầu học:

- Registry Run Keys
- Scheduled Tasks
- Windows Services
- Startup Folder
- DLL Hijacking / Side-loading
- WMI Event Subscription. Pasted text

Quan trọng nhất là **không học chúng như sáu trick riêng biệt**.

Hãy phân loại theo trigger.

---

# 29. Persistence theo trigger

| Technique | Trigger điển hình |
|---|---|
| Run Key | User logon |
| Startup Folder | User logon |
| Scheduled Task | Time/event/logon |
| Service | System/service startup |
| DLL Hijacking | Application start |
| WMI Subscription | WMI event |

Vậy persistence thực chất gồm:

```text
Trigger
   +
Execution mechanism
   +
Payload
```

Ví dụ:

```text
Logon
  ↓
Run Key
  ↓
Program executed
```

---

# 30. Persistence và privilege

Đây là phần syllabus nhấn mạnh rất đúng: phải biết technique yêu cầu quyền nào. Pasted text

Bạn nên luôn classify:

```text
User-level
Local Admin
SYSTEM
Domain-level
```

Một persistence mechanism chạy dưới user context thường chỉ mang lại quyền của user đó.

Persistence không tự động đồng nghĩa:

```text
Privilege Escalation
```

Hai concept này hoàn toàn khác nhau.

---

# 31. Run Keys

Concept:

```text
Registry
   ↓
Logon configuration
   ↓
Program executes when user logs in
```

Điểm cần nhớ:

- thường gắn với logon
- có user scope và machine scope
- registry modification tạo telemetry.

---

# 32. Scheduled Task

Concept:

```text
Task
 ├── Trigger
 └── Action
```

Trigger có thể là:

```text
Time
Logon
Startup
Event
```

Action:

```text
Run program/script
```

Scheduled task rất linh hoạt vì trigger không nhất thiết là thời gian.

---

# 33. Windows Service

Mental model:

```text
Service Control Manager
        ↓
Service configuration
        ↓
Binary/service component
        ↓
Execution
```

Service persistence đặc biệt vì nhiều service có thể chạy dưới privileged account.

Nhưng vì vậy cũng thường tạo telemetry khá rõ.

---

# 34. Startup Folder

Đơn giản:

```text
User logon
   ↓
Startup location processed
   ↓
Configured executable/shortcut runs
```

Dễ hiểu nhưng thường dễ quan sát hơn các mechanism tinh vi khác.

---

# 35. WMI Event Subscription

Concept quan trọng:

```text
Event Filter
     +
Consumer
     +
Binding
```

Tức:

```text
IF event occurs
      ↓
THEN consumer executes
```

Ví dụ tư duy:

```text
Event
   ↓
Filter matches
   ↓
Consumer triggered
   ↓
Code/action executes
```

---

# 36. Persistence từ góc độ Blue Team

Đây là điều operator phải biết.

Persistence thường để lại các loại artifact:

```text
Registry changes
Scheduled-task creation
Service creation
File creation
WMI objects
Process execution
Network connections
```

Red Team giỏi không chỉ hỏi:

> Có chạy được không?

Mà hỏi:

> Defender nhìn thấy gì?

---

# 37. Active Directory fundamentals

Tuần 6 xây foundation AD: domain, DC, forest, OU, GPO và các AD objects. Pasted text

Bạn cần có mental model:

```text
Forest
 └── Domain
      ├── Domain Controllers
      ├── OUs
      │    ├── Users
      │    └── Computers
      ├── Groups
      ├── GPOs
      └── Service Accounts
```

---

# 38. Domain

Domain là administrative/security boundary chứa objects:

```text
Users
Computers
Groups
Policies
```

Có centralized authentication và authorization.

---

# 39. Domain Controller

DC cung cấp các chức năng quan trọng như:

```text
Authentication
Directory services
Policy distribution
Domain information
```

Một workstation domain-joined phụ thuộc rất lớn vào DC và DNS.

---

# 40. Forest

Forest là top-level AD structure.

Có thể chứa:

```text
Forest
 ├── Domain A
 └── Domain B
```

Một điều hay bị nhầm:

> Domain ≠ Forest.

---

# 41. OU

Organizational Unit dùng để tổ chức objects.

Ví dụ:

```text
corp.local
 ├── Employees
 │    ├── Finance
 │    ├── HR
 │    └── IT
 │
 └── Computers
      ├── Workstations
      └── Servers
```

OU cũng thường là nơi GPO được apply.

---

# 42. GPO

Group Policy Object dùng để áp chính sách tập trung.

Ví dụ:

```text
Password/security settings
Firewall
Scripts
Software configuration
System restrictions
```

Mental model:

```text
GPO
 ↓
Linked to AD scope
 ↓
Users/computers
 ↓
Policy applied
```

---

# 43. Groups

Groups rất quan trọng với Red Team vì:

```text
User
  ↓
Group membership
  ↓
Permissions
  ↓
Resource access
```

Do đó group membership thường quan trọng hơn username.

Bạn không chỉ hỏi:

> User là ai?

Mà phải hỏi:

> User thuộc group nào?

---

# 44. Service Accounts

Service Account là account được application/service sử dụng.

Chúng đáng chú ý vì có thể:

```text
Run continuously
Have special privileges
Access databases
Access network resources
```

---

# 45. AD lab architecture

Training yêu cầu xây DC, tạo domain, user/computer/group/GPO và join Windows client vào domain. Pasted text

Một lab tối thiểu có dạng:

```text
              Domain Controller
              DNS + AD DS
                   │
                   │
      ┌────────────┴────────────┐
      │                         │
 Workstation                Server
      │
 Domain User
```

DNS đặc biệt quan trọng.

AD phụ thuộc rất mạnh vào DNS để service discovery.

---

# 46. Enterprise services trong domain

Training yêu cầu biết:

```text
File Share
Print Server
ADCS
Exchange
SharePoint
```

Pasted text

Đây không phải kiến thức phụ.

Chúng tạo **attack surface nội bộ**.

---

# 47. SMB File Share

Concept:

```text
Server
   ↓
SMB Share
   ↓
ACL / Permissions
   ↓
Users / Groups
```

Operator sẽ quan tâm:

```text
Ai access được?
Read hay Write?
Có dữ liệu nhạy cảm?
Có configuration?
Có software deployment files?
```

---

# 48. ADCS

AD Certificate Services gồm:

```text
Certificate Authority
Certificate Templates
Certificates
```

Certificate có thể được sử dụng cho:

```text
Authentication
Encryption
Signing
Machine identity
User identity
```

Một sự thật rất quan trọng:

> Trong Active Directory hiện đại, certificate infrastructure cũng là một phần của identity infrastructure.

Do đó ADCS không nên được xem đơn thuần là “dịch vụ cấp SSL certificate”.

---

# 49. Exchange

Exchange tạo thêm một data source rất lớn:

```text
Emails
Calendars
Address book
Organization structure
```

Trong real-world Red Team, mail infrastructure thường cho biết rất nhiều về quan hệ trong tổ chức.

---

# 50. SharePoint

SharePoint thường chứa:

```text
Documents
Internal portals
Policies
Projects
Team information
```

Vì vậy từ perspective reconnaissance:

```text
File access
+
Identity
+
Enterprise knowledge
```

có thể kết hợp thành intelligence rất có giá trị.

---

# 51. AD Recon Tool

Training yêu cầu viết tool cơ bản để enumerate:

```text
Users
Computers
Groups
```

Pasted text

Điều quan trọng không phải ngôn ngữ programming.

Mục tiêu là hiểu:

```text
Directory query
      ↓
AD objects
      ↓
Properties
      ↓
Relationships
```

Sau này những relationship này chính là nền tảng của các graph-analysis tool như BloodHound.

---

# 52. Internal Reconnaissance – Host

Tuần 7 chuyển từ network bên ngoài sang **situational awareness trên compromised host**. Pasted text

Đây là bước cực kỳ quan trọng.

Ngay khi có session, operator không nên hành động ngẫu nhiên.

Trước tiên phải xây **host profile**.

```text
HOST
 ├── OS
 ├── Software
 ├── Network
 ├── Security Controls
 ├── User
 ├── Privileges
 ├── Domain
 └── Credentials / Secrets
```

---

# 53. Điều đặc biệt trong training của bạn

Syllabus yêu cầu với **mỗi command** phải biết:

```text
Command làm gì?
Command/PowerShell/API nào được gọi?
Output nghĩa là gì?
Log nào được tạo?
```

Pasted text

Đây là một trong những phần quan trọng nhất của toàn bộ training.

Ví dụ operator mindset:

```text
C2 command
    ↓
Underlying mechanism
    ↓
Windows API / command / .NET
    ↓
OS behavior
    ↓
Telemetry
```

Nếu chỉ thuộc command của Havoc hay Adaptix mà không hiểu layer dưới, bạn mới đang học **tool usage**, chưa phải học **Red Team operation**.

---

# 54. OS Enumeration

Training yêu cầu:

```text
Windows version
Build
Architecture
Boot time
```

Pasted text

Tại sao?

### Version + build

Ảnh hưởng đến:

```text
Available features
Security mitigations
Patch assumptions
Compatibility
```

### Architecture

```text
x86 / x64
```

ảnh hưởng payload compatibility.

### Boot time

Có thể cung cấp context về:

```text
Patch/reboot history
System availability
Persistence testing
```

---

# 55. Installed Software

Mục đích không phải lấy một list thật dài.

Bạn cần tìm software có relevance:

```text
Security products
Management tools
Browsers
Database clients
VPN
Developer tools
Backup software
Enterprise agents
```

Sau đó hỏi:

```text
Software này mở thêm attack surface nào?
```

---

# 56. Firewall enumeration

Training yêu cầu biết:

- firewall đang dùng
- Defender Firewall status
- blocked/open ports
- inbound/outbound rules. Pasted text

Hãy nhìn firewall như **network policy engine**:

```text
Traffic
  ↓
Rule evaluation
  ↓
Allow / Block
```

Đừng chỉ hỏi “firewall bật không”.

Quan trọng hơn:

```text
Rule nào đang tồn tại?
Scope?
Direction?
Protocol?
Port?
Application?
```

---

# 57. GPO enumeration

Bạn cần biết:

```text
Machine receives which GPO?
Security policy?
Restrictions?
```

Pasted text

GPO có thể giải thích vì sao:

```text
PowerShell behaves this way
Firewall configured this way
Security settings exist
Scripts run at logon
```

---

# 58. Domain Join Status

Training yêu cầu xác định:

```text
Joined domain?
Which domain?
Which DC?
```

Pasted text

Đây là một branching decision rất lớn.

```text
Not domain joined
      ↓
Mostly local context

Domain joined
      ↓
AD reconnaissance becomes relevant
```

---

# 59. AV / EDR Enumeration

Training yêu cầu xác định:

```text
Product
Version
Local/cloud managed
Status
Protection features
```

Pasted text

Điều bạn cần hiểu:

### Antivirus

Thường tập trung vào:

```text
Malware
Files
Signatures
Behavior
```

### EDR

Có thể collect:

```text
Process creation
Process tree
Network connections
File activity
Registry activity
Memory behaviors
Authentication context
```

Red Team operator phải nghĩ theo telemetry.

---

# 60. Current User Enumeration

Training chia thành:

### System perspective

```text
Username
Local admin?
Privileges?
Integrity level?
Special rights?
Domain roles?
```

### Enterprise perspective

```text
Who is this person?
Department?
Role?
Accessible resources?
```

Pasted text

Đây là distinction rất hay:

```text
Technical Identity
        +
Business Identity
```

Ví dụ:

```text
DOMAIN\alice

Technical:
normal domain user

Business:
finance manager
```

Business role có thể quan trọng hơn technical privilege.

---

# 61. Token / Privilege / Integrity Level

Ba concept dễ bị trộn.

### User

Identity.

### Privileges

Specific OS rights.

### Integrity Level

Process trust level, thường conceptually:

```text
Low
Medium
High
System
```

Một user có thể thuộc Administrators group nhưng process hiện tại chưa chắc đang chạy ở elevated context.

Đây là điểm rất quan trọng trong Windows security.

---

# 62. Credential Hunting

Training đề cập:

- Credential Manager
- saved credentials
- credential files
- browser cache
- config files
- registry
- memory. Pasted text

Hãy hiểu theo **secret storage locations**:

```text
Secrets
 ├── Files
 ├── Registry
 ├── Browser storage
 ├── Credential stores
 ├── Application configuration
 └── Memory
```

Không nên nghĩ credential chỉ là username/password.

Có thể có:

```text
Password
API key
Token
Cookie
Connection string
Private key
Certificate
Session material
```

---

# 63. Credential Manager

Windows cung cấp mechanism để lưu credentials.

Red Team perspective:

```text
What credentials exist?
What resource do they belong to?
What context can access them?
```

Blue Team perspective:

```text
Why are reusable credentials stored?
Can stronger authentication remove them?
```

---

# 64. Browser artifacts

Browser có thể chứa:

```text
Cookies
Sessions
Saved passwords
History
Tokens
Autofill information
```

Một điểm quan trọng:

> Cookie/session token có thể mang authentication state dù bạn không biết plaintext password.

Vì thế identity security không chỉ là password security.

---

# 65. Config files

Đây là một trong những nguồn secret rất thực tế.

Ví dụ class:

```text
Database configs
Application configs
Environment files
Deployment configs
Scripts
Backup configs
```

Các field nhạy cảm có thể gồm:

```text
username=
password=
token=
apikey=
connectionString=
```

---

# 66. Credentials in memory

Ứng dụng đôi khi cần giữ authentication material trong memory để hoạt động.

Mental model:

```text
User authenticates
      ↓
OS/application processes credential
      ↓
Some authentication state may exist in memory
```

Do đó:

```text
Memory protection
Credential isolation
Least privilege
```

rất quan trọng.

---

# 67. Internal Reconnaissance – Network

Tuần 8 mở rộng từ compromised host ra mạng nội bộ. Pasted text

Mental model:

```text
Foothold
   │
   ├── Local network interfaces
   │
   ├── Neighbor hosts
   │
   ├── Internal web services
   │
   ├── SMB
   │
   ├── Databases
   │
   └── Infrastructure
```

Mục tiêu:

> chuyển từ **host awareness** sang **network awareness**.

---

# 68. OS Scanner

OS identification có thể dựa trên nhiều signals.

Conceptually:

```text
Network behavior
Service behavior
Protocol metadata
Banner
```

Nhưng OS fingerprinting thường là **probabilistic**, không phải lúc nào cũng 100% chắc chắn.

---

# 69. NetBIOS Scanner

Training yêu cầu tìm:

```text
NetBIOS name
Workgroup/domain
File shares
```

Pasted text

NetBIOS/SMB information historically rất hữu ích trong Windows network reconnaissance.

Bạn có thể xây mapping:

```text
IP
 ↓
Hostname
 ↓
Domain
 ↓
Shares
```

---

# 70. HTTP/HTTPS Scanner

Training yêu cầu:

```text
Internal web service
HTTP status
Title
Server/technology
```

Pasted text

Một scanner tốt nên biến:

```text
10.0.0.12:443
10.0.0.15:8080
10.0.0.30:80
```

thành context:

```text
10.0.0.12 → VMware management?
10.0.0.15 → Internal application?
10.0.0.30 → Printer/admin portal?
```

Ý nghĩa của service quan trọng hơn con số port.

---

# 71. Port Scanner trong mạng nội bộ

Training yêu cầu xác định TCP/UDP ports, services và banner. Pasted text

Một mental model hữu ích:

```text
IP
 ↓
Port
 ↓
Protocol
 ↓
Service
 ↓
Product
 ↓
Version
 ↓
Role
```

Ví dụ role cuối cùng mới thực sự quan trọng:

```text
Domain Controller
Database
File Server
Admin Console
Developer Server
```

---

# 72. MSSQL Scanner

Training kết thúc ở MSSQL discovery và authentication configuration. Pasted text

Bạn nên hiểu MSSQL có thể sử dụng:

```text
SQL Authentication
Windows Authentication
```

Trong domain environment, Windows Authentication đặc biệt quan trọng vì nó kết nối database access với AD identity.

---

# 73. Tích hợp public tools vào C2

Một yêu cầu rất đáng chú ý của training là:

> Không lấy project GitHub rồi chạy ngay.

Phải:

```text
Read source
Understand functions
Understand parameters
Understand logs
Evaluate safety
Compile/test in lab
```

Pasted text

Đây chính là tư duy đúng.

Một executable có “1000 stars” không có nghĩa là:

```text
safe
stealthy
compatible
trustworthy
```

Bạn phải biết nó thực sự làm gì.

---

# 74. Inline execution và operator maturity

Khi C2 hỗ trợ các mechanism kiểu:

```text
inline execution
BOF
.NET inline execution
modules
```

điều bạn cần hiểu không phải chỉ là syntax.

Hãy phân tích:

```text
Module
   ↓
Execution mechanism
   ↓
Process context
   ↓
Runtime
   ↓
OS API
   ↓
Telemetry
```

Ví dụ .NET tool:

```text
C2
 ↓
.NET runtime
 ↓
Assembly
 ↓
Managed code
 ↓
Windows APIs/network calls
```

---

# 75. Một operator nên nhìn toàn bộ chain như thế nào?

Ví dụ bạn có một foothold:

```text
External Recon
     ↓
Find recruitment portal
     ↓
Initial Access scenario
     ↓
Payload executes
     ↓
Agent connects
     ↓
C2 session
```

Đừng ngay lập tức làm random commands.

Tiếp theo:

```text
Who am I?
 ↓
What machine?
 ↓
What OS?
 ↓
What security controls?
 ↓
Domain joined?
 ↓
What privileges?
 ↓
What network?
```

Sau đó:

```text
Credential exposure?
 ↓
Internal hosts?
 ↓
AD objects?
 ↓
Interesting resources?
```

Đây mới là một operation flow.

---

# 76. Một bảng tổng hợp toàn bộ training

| Phase | Câu hỏi chính |
|---|---|
| External Recon | Target có gì exposed? |
| Initial Access | Làm cách nào đạt execution ban đầu? |
| Payload | Code được thực thi dưới dạng gì? |
| C2 | Làm thế nào duy trì communication? |
| Weaponization | Làm thế nào packaging/execution hoạt động? |
| Persistence | Làm thế nào execution tồn tại sau reboot/logon? |
| AD Fundamentals | Enterprise identity được tổ chức thế nào? |
| Host Recon | Tôi đang ở máy nào, quyền nào? |
| Security Recon | AV/EDR/firewall/GPO ra sao? |
| Credential Recon | Secret nằm ở đâu? |
| Network Recon | Tôi nhìn thấy những host/service nào? |
| AD Recon | Users/computers/groups quan hệ thế nào? |

---

# 77. Bốn lớp kiến thức bạn cần phân biệt khi review

Đây là điểm mình nghĩ **quan trọng nhất**.

## Layer 1 — Tool

Ví dụ:

```text
Nmap
Shodan
Havoc
PowerShell
scanner.exe
```

Đây chỉ là công cụ.

## Layer 2 — Technique

Ví dụ:

```text
Port scanning
DLL Hijacking
Scheduled Task
Credential discovery
```

## Layer 3 — OS mechanism

Ví dụ:

```text
DLL loading
Registry
Service Control Manager
Windows tokens
WMI
SMB
```

## Layer 4 — Objective

Ví dụ:

```text
Recon
Execution
Persistence
Credential Access
Discovery
```

Operator yếu thường học:

```text
Tool → command
```

Operator tốt học:

```text
Objective
   ↓
Technique
   ↓
OS mechanism
   ↓
Tool
```

Nếu tool thay đổi, bạn vẫn làm việc được.

---

# 78. Mapping sang MITRE ATT&CK

Syllabus hiện tại của bạn gần tương ứng với:

```text
Reconnaissance
        ↓
Initial Access
        ↓
Execution
        ↓
Command and Control
        ↓
Persistence
        ↓
Discovery
        ↓
Credential Access
```

Ngoài ra DLL-related techniques giao thoa với:

```text
Defense Evasion
Persistence
Execution
```

Điều này giúp bạn trình bày bài review chuyên nghiệp hơn thay vì:

> Tuần 1 em học Nmap, tuần 2 em học payload...

Nên trình bày:

> Ở phase Reconnaissance, em học cách xây attack surface từ passive OSINT và active enumeration. Sau khi đạt Initial Access, em nghiên cứu payload execution và thiết lập C2. Từ foothold, em thực hiện host và network discovery, đánh giá privilege, security controls, AD context và credential exposure.

Cách thứ hai thể hiện bạn hiểu **operation**, không chỉ nhớ syllabus.

---

# 79. Những phần syllabus của bạn đang thiếu

Đây là những điểm mình nghĩ bạn nên chủ động nhắc trong bài tổng hợp.

### 1. Privilege Escalation

Hiện training có hỏi:

```text
Current user?
Admin?
Integrity level?
Privileges?
```

nhưng chưa có module rõ ràng về:

```text
Current privilege
       ↓
Misconfiguration / vulnerability
       ↓
Higher privilege
```

Đây là bước tự nhiên tiếp theo.

---

## 2. Lateral Movement

Bạn đã học:

```text
Internal host discovery
SMB
MSSQL
AD
Credentials
```

nhưng chưa nối chúng thành:

```text
Machine A
   ↓
Authenticated access
   ↓
Machine B
```

Đây là bước rất quan trọng trong enterprise Red Team.

---

## 3. Network Pivoting

Internal scanning từ foothold dẫn tự nhiên tới:

```text
Pivot
Tunnel
Proxy
Port forwarding
```

Bạn gần đây đang học những phần này trong C2, và về mặt kiến thức nó nên được đặt **sau Internal Recon**, không phải xem là feature riêng của C2.

Mental model:

```text
Operator
   ↓
C2
   ↓
Foothold A
   ↓
Internal network
   ↓
Host B
```

---

## 4. OPSEC

Syllabus có nhắc logs nhưng chưa tách riêng **Operational Security**.

Mỗi action nên đánh giá:

```text
Value
Noise
Risk
Telemetry
Impact
```

Ví dụ:

```text
Can I enumerate everything?
```

không phải câu hỏi đúng.

Đúng hơn là:

```text
What information do I actually need,
and what is the lowest-risk way to obtain it?
```

---

# 80. Red Team ≠ Pentest

Đây cũng là điểm nên chuẩn bị nếu trainer hỏi.

### Pentest

Thường tập trung:

```text
Find vulnerabilities
Exploit vulnerabilities
Demonstrate impact
```

### Red Team

Tập trung hơn vào:

```text
Adversary simulation
Attack paths
Detection capability
People + Process + Technology
Objectives
```

Red Team không nhất thiết cố tìm **nhiều vulnerability nhất**.

Mục tiêu có thể là:

```text
Can the organization detect
and respond to a realistic attack path?
```

---

# 81. Red Team Operator ≠ Malware Developer

Một operator chủ yếu cần:

```text
Infrastructure understanding
Windows/Linux knowledge
Networking
AD
C2
Operational decision making
Recon
Tradecraft
Detection awareness
```

Malware development là một specialization khác.

Bạn cần hiểu payload hoạt động như thế nào, nhưng không có nghĩa toàn bộ Red Team = viết malware.

---

# 82. Red Team Operator ≠ C2 Tool User

Đây có lẽ là bias dễ mắc nhất khi mới training.

Bạn đang học Havoc/Adaptix thì dễ nghĩ:

```text
Biết C2 command
=
Biết Red Team
```

Không đúng.

C2 chỉ là:

```text
Communication
+
Tasking platform
```

Kiến thức thực sự nằm ở:

```text
Windows
Networking
AD
Identity
Protocols
Telemetry
Attack paths
```

Nếu ngày mai đổi:

```text
Havoc → Adaptix → Mythic → Sliver
```

thì logic operation vẫn gần như giữ nguyên.

---

# 83. Khi review từng command C2, hãy dùng format này

Đây là template mình khuyên bạn học.

### 1. Purpose

Command này dùng để làm gì?

### 2. Execution

Agent thực hiện bằng cơ chế gì?

```text
Win32 API?
.NET?
PowerShell?
Native executable?
```

### 3. Context

Nó chạy dưới:

```text
Which user?
Which process?
Which integrity level?
```

### 4. Result

Output có nghĩa gì?

### 5. Telemetry

Có thể tạo:

```text
Process
Network
Registry
File
Authentication
Event log
EDR telemetry
```

### 6. Operational value

Thông tin này quyết định bước nào tiếp theo?

Nếu trả lời được 6 câu trên, bạn thực sự hiểu command.

---

# 84. Những kiến thức nền bạn bắt buộc phải chắc

Để toàn bộ training phía trên không trở thành học thuộc tool, bạn nên chắc các foundation sau:

```text
Networking
├── TCP/IP
├── UDP
├── DNS
├── HTTP/HTTPS
├── SMB
├── ports
├── routing
└── firewall

Windows
├── Process
├── Thread
├── Memory
├── DLL
├── Registry
├── Services
├── Tokens
├── Integrity Level
├── ACL
├── WMI
└── Event Logs

Active Directory
├── Domain
├── Forest
├── DC
├── DNS
├── User
├── Computer
├── Group
├── OU
├── GPO
├── Service Account
├── Kerberos
└── NTLM

C2
├── Teamserver
├── Client
├── Listener
├── Agent
├── Session
├── Task
├── Callback
└── Communication channel
```

Phần **Kerberos/NTLM** đáng chú ý: syllabus hiện tại giới thiệu AD nhưng chưa đi sâu authentication protocol. Đây sẽ là kiến thức rất cần khi bạn tiến sang AD attack paths.

---

# 85. Cách mình khuyên bạn chuẩn bị bài tổng hợp

Bạn đừng học thuộc 100 command.

Hãy đảm bảo có thể tự vẽ từ trí nhớ sơ đồ này:

```text
Internet
   │
   ▼
External Recon
   │
   ▼
Attack Surface
   │
   ▼
Initial Access
   │
   ▼
Payload Execution
   │
   ▼
C2 Session
   │
   ▼
Host Recon
   ├── OS
   ├── User
   ├── Privilege
   ├── AV/EDR
   ├── Firewall
   ├── GPO
   └── Domain
   │
   ▼
Persistence
   │
   ▼
Credential Discovery
   │
   ▼
Internal Network Recon
   ├── Hosts
   ├── Ports
   ├── HTTP
   ├── SMB
   └── MSSQL
   │
   ▼
AD Recon
   ├── Users
   ├── Computers
   ├── Groups
   └── Policies
   │
   ▼
Privilege Escalation
   │
   ▼
Lateral Movement
   │
   ▼
Objective
```

Hai bước cuối mình thêm để hoàn chỉnh lifecycle; chúng **chưa phải nội dung chính trong file training hiện tại**.

---

# 86. Nếu trainer hỏi: “Sau khi có session thì em làm gì?”

Đây là câu bạn nên trả lời theo logic:

```text
1. Xác định session context
        ↓
2. Enumerate host
        ↓
3. Enumerate current identity và privileges
        ↓
4. Xác định security controls
        ↓
5. Xác định domain context
        ↓
6. Thu thập network information
        ↓
7. Xác định credential/secret exposure
        ↓
8. Discover internal hosts/services
        ↓
9. Enumerate AD nếu domain joined
        ↓
10. Dựa trên dữ liệu để quyết định bước tiếp theo
```

Không nên trả lời:

> “Em chạy whoami, ipconfig rồi scan.”

Vì đó là **command-oriented**, không phải **objective-oriented**.

---
