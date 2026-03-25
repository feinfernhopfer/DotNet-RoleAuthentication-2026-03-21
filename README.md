## Role-based Authentication for Dot Net 10 as of March 2026

This is a small set of files that have been edited to give Authentication and Authorization features for an ASP.NET web app:
- [**Blazor-Authentication v10.0.201**](https://github.com/feinfernhopfer/DotNet-RoleAuthentication-2026-03-21) - Files supporting an ASP.NET Core hosted Blazor web app

These files demonstrate how to configure User Authentication:
- Blazor Web App (using Razor pages)
- Using EntityFramework and SQL Server for data access
- User management with ASP.NET Core Identity

## Summary

Set up a Blazor web app to use role-based authentication in the development environment. 

Coding for use and testing on the development machine with the IIS web server that comes with the Visual Studio IDE. 

Note - The code and script used here worked well with the Community Edition(s) of the Microsoft development tools. 

An easy and simple 20-minute process.


##### Manual Editing required (Visual Studio): 

1 - Create a new Blazor Web App using 'Individual Accounts' for Authentication Type (and 'Include sample pages')

2 - Edit the Blazor App's /appsettings.json file to set the connection string for data access

3 - Edit the Blazor App's /Program.cs file to use the Authentication and Authorization features

4 - Edit the target pages to add an AuthorizeView control

##### Automated Editing required (Visual Studio): 

5 - Use the Package Manager Console to Create and Configure an ASP.NET database (for the Identity and EntityFrameworkCore features)

##### Manual Editing required: Editing required (Visual Studio): 

6 - Use the Visual Studio (integral debugging) Web Browser to Create New Users

##### Manual Editing required: Editing required (SSMS): 

7 - Use SQL Server Management Studio to Create New Roles (for Users)

8 - Use SQL Server Management Studio to Create New Assignment of Roles to Users

##### Testing:  (Visual Studio): 

9 - Use the Visual Studio (integral) Web Browser to test for functionality when using Authentication and Authorization features in a Blazor Web App

## Why this article?

Q. 	Why this article? 
	
A. 	Multiple reasons exist for why this article was written and published: 
	
1. The Microsoft documentation team cannot publish updates fast enough to keep up with their in-house development team. 
	
	a. Updates to Dot NET often require updates to coding for use with Blazor Web Apps. 
		
	b. At least 19 changes in the past 8 years have broken legacy code for Microsoft Dot NET, and some of those changes have affected Role-based Authentication. 
			
2. Online postings to industry websites often contain outdated information, which was once helpful, but are now of no use at all; yet these pages are never taken out of publication, which causes a lot of frustration for web developers.
	
3. The ASP.NET database model has changed in recent months (online tutorials from August to October 2025 are no longer useful for Authentication/Authorization).
	
4. This article brings coding for [Role-based Authentication for Blazor Web Apps with Microsoft Dot NET 10] current as of 2026-03-21. 

## Prerequisites

Start with a ready-to-go SQL Server database containing the most recent build of ASPNET dbo objects. 
	If not, then quickly build it with the Visual Studio automated tools.

Yes, AI at its finest, as it is helping you. 
The tools are quick and accurate. 
The automated tools eliminate many hours of correcting human errors due to hand coding of such complex and feature-rich libraries.

Note - The data model has changed a few times, in recent months. 
The Update-database Functionality will ensure that the database is fully prepared for use with Blazor Authentication.

The following tools are necessary:
1. Windows 11 Pro, v25H2 (with current Windows Update applied)
2. [Install .NET 10.0.201](https://dotnet.microsoft.com/en-us/download/dotnet/10.0)
3. [Install SQL Server Management Studio, v22.4.1](https://learn.microsoft.com/en-us/ssms/install/install)
4. [Install Microsoft SQL Server 2025, Standard Developer Edition, 17.0.1105.2 (X64)](https://www.microsoft.com/en-us/sql-server/sql-server-downloads)
5. [Install Visual Studio 2026, v18.4.1](https://visualstudio.microsoft.com/downloads/).

### Database

The application uses SQL Server and entity framework.

### PROCESS: 

1. Use Visual Studio to create the web app. 
	A.	From the Visual Studio IDE
		1) NEW: Create a new Blazor web app.
		2) Set the name for the new web app (for example, <myBlazorApp>).
		3) Select 'Individual Accounts' as Authentication type (necessary for user authentication).
		4) Select 'Include sample pages' (necessary for adding new users with the 'Register' page).
		5) The IDE creates and opens the new Blazor Web App.
		
	B.	Set the connection string for the Microsoft SQL Server database (in the /appsettings.json file).
		NOTE - SQL Server installed on the development machine. 
		NOTE - Be sure to include, 'TrustServerCertificate=true;' because you are using a bogus certificate for development.
		1) Edit the (/appsettings.json) file.
		
				"ConnectionStrings": {"DefaultConnection": 
					"Server=myDevelpmentPC;Database=aspnet-myBlazorApp-2026-03-21;
					TrustServerCertificate=true;
					Trusted_Connection=True;
					MultipleActiveResultSets=true"},

	C. Build the ASP.NET database using the automated tools in Visual Studio. 
		1)	Look for the Tools menu located at the upper menu bar in Visual Studio.
				Tools > Nuget Package Manager > Package Manager Console
		2)	The command line tool (PMC) opens at the center bottom pane of the IDE.
			Use the Update-database feature to create and configure the ASP.NET database for use with Blazor Authentication and Authorization. 
				>Update-Database
			a)	Expect a 'Build succeeded' message followed by a brief pause, 
				then wait for the success message, 'Done' 
				(the Happy Path, else correct whatever went wrong and then repeat using the PMC).
			b)	The PMC indicates that database is ready for use ('Done.' message).
	
	D.	Configure the main web app file (/Program.cs). Add 4 lines of code. 

			~
			builder.Services.AddAuthorizationCore(); // RALPH, per AI on Bing.
			~
				.AddRoles<IdentityRole>() // Enable role support. RALPH, per AI on Bing. 
			~
			app.UseAuthentication(); // RALPH, per AI on Bing.
			app.UseAuthorization(); // RALPH, per AI on Bing.
			~

	E.	Add scripting to create the desired form controls for the target web page (/Components/Pages/Weather.razor).
		For this article, add a AuthorizeView form control to vary the page content per developer-defined Role-based security. 

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
		
F. Save the changes.
			1) Clean Build.
			2) Rebuild.
			3) Save All.

G.	Create the new Users.
	Use Visual Studio debugging browser to create the new Users. 
	1)	Run the app in the IDE.
	2)	Use the REGISTER page to create a new user. 
		a) Enter the user specifics. 
		b) Click here to confirm your account.
		c) Log in.
		d) Log out.
H.	Repeat the steps for each new record to be created in the [AspNetUsers]::datatable.

2. Use SQL Server Management Studio to add functionality to the Web App. 
	A. Create the new Roles.
	Use SQL Server Management Studio to create the new Roles.
	Use the command line window to write the TSQL script to create the new records for Roles.
		1) Open SSMS.
		2) Use the command line window to write the TSQL script to create the new Roles.
			a)	In the Explorer pane located at the left of the IDE, right-click on the [AspNetRoles]::datatable.
			b)	Select "Script Table as"...
			c)	Select "Insert to"...
			d)	Select "New Query Editor Window".
			e)	Modify the template to match the specifics for creating a new record.
			
							USE [aspnet-myBlazerApp-2026-03-21]		
							GO
							INSERT INTO [dbo].[AspNetRoles]
									( [Id]
									 ,[Name]
								     ,[NormalizedName])
							   VALUES
								   ( N''
									,N''
									,N'')
						   GO

				f)	Execute the query.
				g)	Repeat the steps for each new record to be created in the [AspNetRoles]::datatable.
					
B.	Assign Roles to Users.
	Use SQL Server Management Studio to assign Roles to Users.
	Use the command line window to write the TSQL script to create the new records for User-Roles.
	1)	In the Explorer pane located at the left of the IDE, right-click on the [AspNetUserRoles]::datatable.
	2).	Select "Script Table as"...
	3).	Select "Insert to"...
	4).	Select "New Query Editor Window".
		a)	Prepare a new INSERT query. 
			i)	A template will appear in the window. Modify the template to match your needs, then create the new record.
			ii)	In the Query Editor, modify the provided script to suit your specific needs to create the new record.
			iii)	Gather the specific text strings to modify the template.
				A)	Copy/Paste the User ID (a string - nvarchar datatype - for the GUID that identifies the particular User).
					1)	Open a select query to capture the particular ID for the User.
					2)	Right click on the [AspNetUsers]::datatable. 
					3)	Select "Select Top 1000 Rows". A datagrid will then populate with the records already in the datatable.
					4)	Left-click on the particular [Id] of the User, then right-click and select, "Copy".
					5)	Right-click to paste the [Id] string into the query window of the [newInsertQuery.sql] file (N'<ParticularUserGuid>'). 
				B)	Copy/Paste the Role ID (a string - nvarchar datatype that identifies the particular Role). 
					1)	Open a select query to capture the particular ID for the Role.
					2)	Right click on the [AspNetRoles]::datatable. 
					3)	Select "Select Top 1000 Rows". A datagrid will then populate with the records already in the datatable.
					4)	Left-click on the particular [Id] of the Role, then right-click and select, "Copy".
					5)	Right-click to paste the [Id] string into the query window of the [newInsertQuery.sql] file (N'<ParticularRoleNameId>'). 
				iv)	Execute the query.
					
					USE [aspnet-myBlazerApp-2026-03-21]
					GO
					INSERT INTO [dbo].[AspNetUserRoles]
							   ([UserId]
							   ,[RoleId])
						 VALUES
							   (N''
							   ,N'')
					GO

			v)	Repeat the steps for each new record to be created in the [AspNetUserRoles]::datatable.

3.	The Blazor web app is now ready for use/testing.

4. Use Visual Studio to test the code for functionality. 
	A. Use the Visual Studio (integral debugging) Web Browser to verify functionality: 
		1) User log-in.
		2) User authorization. 


###	Necessary Files

		/appsettings.json
		/Program.cs
		/Components/Pages/Weather.razor
		CreateNewRole.sql
		CreateAssignRoleToUser.sql
		readme.txt

### References

- [Info about editing the main web app file (/Program.cs) for use with Role-based Authentication]
	(https://www.bing.com/search?q=builder.Services.AddAuthorization)

-  [Info about AuthorizeView for the web page (/Weather.razor)] (https://www.bing.com/search?q=AuthorizeView)
