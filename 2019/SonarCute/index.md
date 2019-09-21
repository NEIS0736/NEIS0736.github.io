# **"CI/CD We Can"** *by #SonarCute*!
---

![](ScopePresentation.jpg "by Khun Ardnarong Boonkerd")

# **CI/CD Process**
![](CICD_Process01.png)


---

## **CI (continuous integration)**

### **Step 1. Create GigHub Project, Create GitLab CI/CD Project, Develop source code**
* **(Maintainer)**

	* Create GitHub Project
		- สร้าง New repository บน GitHub
		- เพิ่ม Collaborators ส่วนของผู้ใช้งานหรือ ที่จะมีสิทธิ์เข้ามาทำอะไรได้บ้าง โดยปกติจะเป็น Dev
	* Create GitLab CI/CD Project
		- Login GitLab
	- Create new project
	- Select CI/CD for external repo > Select GitHub 
	![](CICD_creategitlabproject01.png)
	- Input GitHub Personal Access Token 
	![](CICD_creategitlabproject02.png)
	- Click Generate new token 
	![](CICD_creategitlabproject03.png)
	- Select scopes 
	![](CICD_creategitlabproject04.png)
	- Copy tokens 
	![](CICD_creategitlabproject05.png)
	- Back to GitLab and input access token 
	![](CICD_creategitlabproject06.png)
	- Select project to connect (click connect) 
	![](CICD_creategitlabproject07.png)
	- (Github) create .gitlab-ci.yml 
	![](CICD_creategitlabproject08.png)
	- หลังจากที่ Commit แล้วระบบจะสั่งการให้ GitLab CI/CD ทำงาน
	![](CICD_creategitlabproject09.png) 
- Ref.https://ardnarong.github.io/neis0736-cicd/Using%20GitLab%20CI-CD%20with%20a%20GitHub%20repository/
	
* **(Developer)**

	* login GitHub และใช้ sonar lint ในการพัฒนาซอสโค้ด
		- Developer พัฒนาโปรแกรมโดย VS Code เป็น IDE ที่รองรับ SonarLint 
		![](CICD_codewithsonar01.png)
		- เสร็จแล้วทำการ git Commit แล้ว git Push 
		![](CICD_codewithsonar02.png)
		- จากนั้น CI จะเริ่มทำงานตาม ไฟล์ .gitlab-ci.yml
		![](CICD_codewithsonar03.png)
		![](CICD_codewithsonar04.png)
		- ไปดูผลการ Review source code ที่ https://sonarcloud.io 
		![](CICD_codewithsonar05.png)
		- Ref.https://ardnarong.github.io/neis0736-cicd/Improving%20code%20quality%20with%20SonarQube/


---

### **Step 2. Send GitLab Runner info, Prepare server and install Gitlab Runner**
* **(Maintainer)**
	* Send Gitlab Runner info เพื่อให้ system admin ทำการ install Gitlab runner
		- Login GitLab
	- Go to setting menu > CI/CD > Runner and click expand 
	![](CICD_gitlabtoken01.png)
	- ส่งข้อมูลด้านล่างนี้ให้กับ System Admin เพื่อติดตั้ง GitLab Runner บน Server 
	![](CICD_gitlabtoken02.png)
	- ทำการปิด Shared Runners โดยคลิกที่ Disable Shared Runners 
	![](CICD_gitlabtoken03.png)
	- Ref.https://ardnarong.github.io/neis0736-cicd/Maintainer%20send%20GitLab%20runner%20token%20to%20System%20Admin/

* **(System Engineer)**
	* Prepare Server and install Gitlab Runner
		- IP : aaa.bbb.ccc.ddd
		- username : xxx
		- password : yyy
		- **_Install Docker and Docker Compose -> Done_**
			- https://github.com/NaturalHistoryMuseum/scratchpads2/wiki/Install-Docker-and-Docker-Compose-(Centos-7)
		- **_Install GitLab Runner -> Done_**
			- https://docs.gitlab.com/runner/install/linux-repository.html
		- **_Registering Runners -> Done_**
			- https://docs.gitlab.com/runner/register/index.html
		- **_Grant sudo permissions_**
			- You can grant sudo permissions to the gitlab-runner user as this is who is executing the build script.
				- $ sudo usermod -a -G sudo gitlab-runner
			- You now have to remove the password restriction for sudo for the gitlab-runner user.
			- Start the sudo editor with
				- $ sudo visudo Now add the following to the bottom of the file
			- gitlab-runner ALL=(ALL) NOPASSWD: ALL
		- Ref.https://ardnarong.github.io/neis0736-cicd/System%20Admin%20Prepare%20Server/

---

### **Step 3. Connect GitHub กับ Sonar Cloud**

* **(Maintainer)**
	* ทำการเชื่อมต่อ GitHub กับ Sonar Cloud
		- Login Sounar cloud with github account 
	- Click Create a new project 
	![](CICD_connectgitsonar01.png)
	- Click Choose and organization on GitHub 
	![](CICD_connectgitsonar02.png)
	- Install SonarCloud on github account 
	![](CICD_connectgitsonar03.png)
	- Click Continue 
	![](CICD_connectgitsonar04.png)
	![](CICD_connectgitsonar05.png)
	- Choose a Free plan
	- Select github repositories for analysis 
	![](CICD_connectgitsonar06.png)
	- Select “With other CI tools” 
	![](CICD_connectgitsonar07.png)
	![](CICD_connectgitsonar08.png)
	- Select build technology
	- Sonarcloud show script for install 
	![](CICD_connectgitsonar09.png)
	- Ref.https://ardnarong.github.io/neis0736-cicd/github-and-sonarcloud/


---

### **Step 4. Discusstion process CI stage (Build, Test, Deploy)**

* **(Maintainer)(System Engineer)(Developer)**
	* Process CI Stage
		- ใช้ Docker(docker-compose.yml) test (Review code ) ใช้ Sonar Cloud ทั้งหมดทำงานด้วย .gitlac-ci.ymlDeveloper จึง Commit และ push code ที่ทำการแก้ไขแล้วเสร็จจากการตรวจของ Sonar Cloud ไปให้ Gitlab CI/CD project
		![](CICD_cistatge01.png)
		- Ref.https://ardnarong.github.io/neis0736-cicd/Improving%20code%20quality%20with%20SonarQube/images/img%20(4).png


---

### **Step 5. UAT**

* **(Quality Engineer)**
	* ทำ UAT หลังจาก Develop ผ่านการแก้ไขจะเข้าสู่กระบวนการ Change เพื่อทำการ Deploy production
		- Test Case-Add User
			- Preconditons :
				1. ระบบที่เปิดให้บริการ
				2. ข้อมูลของ User ที่จะต้องทำการ Add
			- Aciton : 
				1. Login เข้าสู่ระบบ
				2. เข้าเมนู User
				3. กดปุ่ม “+Add New”
				4. กรอกข้อมูลของ User ที่ต้องทำการ Add
				5. กดปุ่ม “Submit” เพื่อเพิ่ม User
			- Input : 
				1. Full name
				2. Email Address
				3. Default Password
				4. Mobile Number
				5. Role
			- Expected Result :
				1. เพิ่มข้อมูลของ User ที่ต้องการ Add
					![](CICD_adduser01.png)
				2. สามารถ Add User ได้โดยไม่เกิด Error
					![](CICD_adduser02.png)
				3. มี User ที่ทำการเพิ่ม อยู่ในระบบ
					![](CICD_adduser03.png)

		- Test Case-Delete User
			- Preconditons :
				1. ระบบที่เปิดให้บริการ
				2. User ในระบบ
			- Aciton : 
				1. Login เข้าสู่ระบบ
				2. เข้าเมนู User
				3. เลือก User ที่ต้องการแก้ไขข้อมูล
				4. กดปุ่ม Delete เพื่อลบ User
			- Input : 
				1. User
			- Expected Result :
				1. แสดงข้อความ “Are you sure to delete this user ?” ก่อนทำการลบ User
					![](CICD_deluser01.png)
				2. แสดงข้อความ “User successfully deleted” เพื่อให้ทราบว่าทำการลบ User สำเร็จ
					![](CICD_deluser02.png)
				3. ในระบบไม่มี User ที่ทำการลบไปแล้ว
					![](CICD_deluser03.png)
		
		- Test Case-Edit User
		
			- Preconditons :
				1. ระบบที่เปิดให้บริการ
				2. User ในระบบที่จะต้องทำการแก้ไข
				3. ข้อมูลเบอร์โทรศัพท์ใหม่ของ User ที่จะต้องแก้ไข
			- Aciton : 
				1. Login เข้าสู่ระบบ
				2. เข้าเมนู User
				3. เลือก User ทำต้องการแก้ไขข้อมูล
				4. เปลี่ยนข้อมูลเบอร์โทรศัพท์
				5. กดปุ่ม Submit เพื่อแก้ไขข้อมูล
			- Input : 
				1. Mobile Number
			- Expected Result :
				1. แสดงข้อมูล User ก่อนทำการแก้ไขข้อมูล
					![](CICD_edituser01.png)
				2. สามารถแก้ไขข้อมูลของ User ได้
					![](CICD_edituser02.png)
				3. ข้อมูลในระบบมีการเปลี่ยนแปลง
					![](CICD_edituser03.png)
				
		- Test Case-Login Fail
			- Preconditons :
				1. Email Address
				2. Password
			- Aciton : 
				1. กรอก Email Address ผิดในช่อง
				2. กรอก Password ผิดในช่อง
				3. กดปุ่ม Sign In
			- Input : 
				1. Email และ Password
			- Expected Result :
				1. ไม่สามารถเข้าใช้งานได้ พร้อมทั้งมีแจ้งชื่อผู้ใช้งานหรือรหัสผ่านผิด
					![](CICD_loginf01.png)
					![](CICD_loginf02.png)
		
		- Test Case-Login Success
			- Preconditons :
				1. Email Address
				2. Password
			- Aciton : 
				1. กรอก Email Address ผิดในช่อง
				2. กรอก Password ผิดในช่อง
				3. กดปุ่ม Sign In
			- Input : 
				1. Email และ Password
			- Expected Result :
				1. สามารถ Login เข้าใช้งานได้สำเร็จ
			- Post Conditions : สามารถเข้าสู่ระบบและใช้งานระบบได้อย่างปรกติ
					![](CICD_logins01.png)
					![](CICD_logins02.png)

---
# **CD (continuous deployment)**

### **Step 6. Deploy and Monitoring**

* **(System Engineer)**
	* Deploy production 
		- Login GitLab
		- Goto menu CI/CD -> Pipeline
		- On stage deploy prodcutin > Click Play 
		![](CICD_deploy01.PNG)
		- System deploy passed 
		![](CICD_deploy02.PNG)
		- Visite website 
		![](CICD_deploy03.PNG)
		- Ref.https://ardnarong.github.io/neis0736-cicd/deploy/

	
	
	* Monitoring
		- **_prometheus ใช้สำหรับการมอนิเตอร์ docker, service ต่าง ๆ ของระบบ_**
			- https://prometheus.io
		- **_Zabbix ใช้สำหรับมอนิเตอร์ฮาร์ดแวร์_** 
			- https://www.zabbix.com
		- **_Elastic Elastic SIEM ใช้สำหรับมอนิเตอร์ Event ต่าง ๆ_**
			- https://www.elastic.co/products/siem


---

* **Team Member**

	- Ardnarong Boonkerd (Development Leader)
	- Angkarn Pummarin (Project Facilitator)
	- Suparath Suwannakorth (Maintainer)
	- Sirimongkol Wongfu (System Admin)
	- Raksapon Leelachat (System Admin)
	- Pongpat Petchai (Quality Engineer)
	- Tanapad Onsri (Quality Engineer)
	- Maykin Warasart (Project Sponsor)
![](Group_CI-CD01.jpg)


---

##### **[Software Security - NEIS0736](../) (2019)**!
