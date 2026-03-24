/// -------------------------------- 
https://github.com/feinfernhopfer/DotNet-RoleAuthentication-2026-03-21
https://github.com/feinfernhopfer/DotNet-RoleAuthentication-2026-03-21.git

Description: Role-based Authentication for Dot Net 10 as of March 2026



/// ----------------------- 
readme.txt

"Role-based User Authentication with Microsoft Dot NET 10 for Blazor Web Apps in the Development Environment, as of 2026-03-21", Ralph Counts.
SDK version 10.0.201, released March 12, 2026
Blazor Web Apps; 
Visual Studio 2026; 
Microsoft Dot NET 10; 
SQL Server Management Studio; 
Microsoft SQL Server 2025; 
C#; 
TSQL; 



/// ----------------------- 
"Role-based User Authentication with Microsoft Dot NET 10 for Blazor Web Apps in the Development Environment, as of 2026-03-21", Ralph Counts.
	Q. 	Why this article? 
	A. 	Multiple reasons exist for why this article was written and published: 
		1. The Microsoft documentation team cannot publish updates fast enough to keep up with their in-house development team. 
			a. Updates to Dot NET often require updates to coding for use with Blazor Web Apps. 
			b. At least 19 changes in the past 8 years have broken legacy code for Microsoft Dot NET, and some of those changes have affected Role-based Authentication. 
		2. Online postings to industry websites often contain outdated information, which was once helpful, but are now of no use at all; yet these pages are never taken out of publication, which causes a lot of frustration for web developers. 
		3. This article brings coding for [Role-based Authentication for Blazor Web Apps with Microsoft Dot NET 10] current as of 2026-03-21. 

SUMMARY: 
	Set up a Blazor web app to use role-based authentication in the development environment. 
	Coding for use and testing on the development machine with the IIS web server that comes with the Visual Studio IDE. 
	Note - The code and script used here worked well with the Community Edition(s) of the Microsoft development tools. 
	An easy and simple 20-minute process. 
		1. Open Visual Studio to create a new Blazor App using Individual Accounts and include the sample pages.
		2. Set the connection string for the Microsoft SQL Server (edit the /appsettings.json file).
		3. Use the Visual Studio command line prompt to create the Dot NET database (Tools > Nuget Package Manager > Package Manager Console, Update-Database). 
		4. Add four (4) lines of code (for User Authentication and Authorization) to use these features to help secure your web app (edit the /Program.cs file). 
		5. Add two lines (2) of code (for the directives) to each page that requires access controls (User Authentication and Authorization), then add the form control, 'AuthorizeView' (edit the desired Blazor page file). 
		6. Create new Users with the Visual Studio debugging browser (click on the 'Register' form). 
		7. Use SQL Server Management Studio to create the Roles and to assign Roles to Users. 
		8. Test the web app in the development environment, specifically for use with Role-based Authentication. 

CONDITIONS:
1.	Set up the development environment.
	Changes are made frequently, so these code snippets are highly perishable/time-sensitive.
		Note - The data model has changed a few times, in recent months. 
		Caution - No one can be sure of how long this info will be useful.
	Equipment - Development Environment
		Operating System	Windows 11 Pro, v25H2
		Libraries	Microsoft .NET Framework, v10.0 LTS
		Database Tools	SQL Server Management Studio, v22.4.1
		Database	Microsoft SQL Server 2025, Standard Developer Edition, 17.0.1105.2 (X64)
		IDE	Visual Studio 2026, v18.4.1
		Tested/Authored Date	2026-03-21

	Attached Files
		/appsettings.json
		/Program.cs
		/Components/Pages/Weather.razor
		CreateNewRole.sql
		CreateAssignRoleToUser.sql
		readme.txt

2.	Start with a ready-to-go SQL Server database containing the most recent build of ASPNET dbo objects. 
	If not, then manually build it with the Visual Studio tools - See the following step for further details.
	The following tools are necessary: 
	A.	Microsoft Dot NET 10 SDKs installed on the development machine (which will install by default when updating the following apps). 
	B.	Microsoft SQL Server 2025 (already installed on the development machine, per the connection string). 
	C.	SQL Server Management Studio (already installed on the development machine). 
	D.	Visual Studio 2026 (already installed on the development machine). 

3.	If needed, and assume needed (the Dot NET database for authentication has changed recently) - Manually use the NuGet packages in Visual Studio to automatically build the ASP.NET database. 
	A.	Open the Visual Studio IDE. 
	B.	Create a NEW Blazor Web App. 
	C.	Set the connection string for the Microsoft SQL Server database (in the /appsettings.json file). 
	D.	Build the database using the automated tools. 
		Yes, AI at its finest, as it is helping you. 
		The tools are quick and accurate. 
		The automated tools eliminate many hours of correcting human errors due to hand coding of such complex and feature-rich libraries.
		1)	Look for the Tools menu located at the upper menu bar in Visual Studio.
				Tools > Nuget Package Manager > Package Manager Console
		2)	The command line tool opens at the center bottom pane of the IDE.
				>Update-Database
		3)	Expect a 'Build succeeded' message followed by a brief pause, then wait for the success message, 'Done' (the Happy Path, else correct whatever went wrong and then repeat using the PMC).
		4)	Once the PMC indicates that database is ready, then begin with the app-specific coding that will be done manually (using SQL Server Management Studio).
4.	Use Visual Studio to manually code the desired controls that are specific to the web app. 
	A.	From the Visual Studio IDE
		NEW: Create a new Blazor web app.
		Set the name for the new web app (for example, <myBlazorApp>).
		Select 'Individual Accounts' as Authentication type (necessary for user authentication).
		Select 'Include sample pages' (necessary for adding new users with the 'Register' page).
	B.	Set the connection string for the Microsoft SQL Server database (in the /appsettings.json file).
		NOTE - SQL Server installed on the development machine. 
		NOTE - Be sure to include, 'TrustServerCertificate=true;' because you are using a bogus certificate for development.

  "ConnectionStrings": {
    "DefaultConnection": "Server=myDevelpmentPC;Database=aspnet-myBlazorApp-2026-03-21;TrustServerCertificate=true;Trusted_Connection=True;MultipleActiveResultSets=true"
  },


	C.	Configure the main web app file (/Program.cs). Add 4 lines of code. 

builder.Services.AddAuthorizationCore(); // RALPH, per AI on Bing.

    .AddRoles<IdentityRole>() // Enable role support. RALPH, per AI on Bing. 

app.UseAuthentication(); // RALPH, per AI on Bing.
app.UseAuthorization(); // RALPH, per AI on Bing.


	D.	Add scripting to create the desired form controls for the web page (/Components/Pages/Weather.razor).
@page "/weather"
@attribute [StreamRendering]

@using Microsoft.AspNetCore.Identity;	 
@using myBlazorApp.Data;
	
<AuthorizeView Roles="Analysts,Developers,SalesClerks">
    <Authorized>
        <p>Welcome, authorized user.</p>
        <p>Protected content goes here!</p>
    </Authorized>
    <NotAuthorized>
        <p>You are not authorized to view this content.</p>
    </NotAuthorized>
</AuthorizeView>


5.	Create the new Users.
	Use Visual Studio debugging browser to create the new Users. 
	A.	Run the app in the IDE.
	B.	Use the REGISTER page to create a new user. 
		1) Enter the user specifics. 
		2) Click here to confirm your account.
		3) Log in.
		4) Log out.
	C.	Repeat the steps for each new record to be created in the [AspNetUsers]::datatable.

6.	Create the new Roles.
	Use SQL Server Management Studio to create the new Roles.
	Use the command line window to write the TSQL script to create the new records for Roles.
	A.	Open SSMS.
	B.	Use the command line window to write the TSQL script to create the new Roles.
		1)	In the Explorer pane located at the left of the IDE, right-click on the [AspNetRoles]::datatable.
		2)	Select "Script Table as"...
		3)	Select "Insert to"...
		4)	Select "New Query Editor Window".
		5)	Modify the template to match the specifics for creating a new record.
		6)	Execute the query.
		7)	Repeat the steps for each new record to be created in the [AspNetRoles]::datatable.
7.	Assign Roles to Users.
	Use SQL Server Management Studio to assign Roles to Users.
	Use the command line window to write the TSQL script to create the new records for User-Roles.
	A.	In the Explorer pane located at the left of the IDE, right-click on the [AspNetUserRoles]::datatable.
	B.	Select "Script Table as"...
	C.	Select "Insert to"...
	D.	Select "New Query Editor Window".
		1)	Prepare a new INSERT query. 
			a)	A template will appear in the window. Modify the template to match your needs, then create the new record.
			b)	In the Query Editor, modify the provided script to suit your specific needs to create the new record.
			c)	Gather the specific text strings to modify the template.
				i)	Copy/Paste the User ID (a string - nvarchar datatype - for the GUID that identifies the particular User).
					aa)	Open a select query to capture the particular ID for the User.
					bb)	Right click on the [AspNetUsers]::datatable. 
					cc)	Select "Select Top 1000 Rows". A datagrid will then populate with the records already in the datatable.
					dd)	Left-click on the particular [Id] of the User, then right-click and select, "Copy".
					ee)	Right-click to paste the [Id] string into the query window of the [newInsertQuery.sql] file (N'<ParticularUserGuid>'). 
				ii)	Copy/Paste the Role ID (a string - nvarchar datatype that identifies the particular Role). 
					aa)	Open a select query to capture the particular ID for the Role.
					bb)	Right click on the [AspNetRoles]::datatable. 
					cc)	Select "Select Top 1000 Rows". A datagrid will then populate with the records already in the datatable.
					dd)	Left-click on the particular [Id] of the Role, then right-click and select, "Copy".
					ee)	Right-click to paste the [Id] string into the query window of the [newInsertQuery.sql] file (N'<ParticularRoleNameId>'). 
			d)	Execute the query.
			e)	Repeat the steps for each new record to be created in the [AspNetUserRoles]::datatable.

8.	The Blazor web app is now ready for use/testing.


REFERENCES:
1. Info about editing the main web app file (/Program.cs) for use with Role-based Authentication.
	https://www.bing.com/search?q=builder.Services.AddAuthorization

2.	Info about AuthorizeView for the web page (/Weather.razor).
	https://www.bing.com/search?q=AuthorizeView


SAMPLE QUERIES: 

USE [aspnet-myBlazerApp-2026-03-21]
GO
INSERT INTO [dbo].[AspNetRoles]
           ([Id]
           ,[Name]
           ,[NormalizedName])
     VALUES
           (N''
           ,N''
           ,N'')
GO

USE [aspnet-myBlazerApp-2026-03-21]
GO
INSERT INTO [dbo].[AspNetUserRoles]
           ([UserId]
           ,[RoleId])
     VALUES
           (N''
           ,N'')
GO




/// -------------------------- 
