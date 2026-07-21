# Student Registration Web App — Authentication & Authorization Assignment

ASP.NET Core MVC application with role-based authentication and authorization using ASP.NET Core Identity.

## Features

### Authentication
- ASP.NET Core Identity (Individual Accounts)
- User Registration & Login
- New users auto-assigned "Student" role
- Logged-in user's email displayed in navbar
- Welcome message on successful login

### Authorization (Role-Based)
- **Admin**: CRUD courses, view all students
- **Student**: View courses, register for ONE course, view/edit own profile
- Students CANNOT: create/edit/delete courses, view other students, see student list
- URL-based access prevented via `[Authorize]` attributes

### Navigation Menu (Role-Based)
- **Anonymous**: Home, Courses, Register, Login
- **Student**: Home, Courses, Register for Course, My Profile, Logout
- **Admin**: Home, Courses, Students, Logout

### Security
- `[AllowAnonymous]` on Home and Course List
- `[Authorize(Roles = "Student")]` on Profile and Course Registration
- `[Authorize(Roles = "Admin")]` on Course CRUD and Students list
- Access Denied page for unauthorized access attempts

## Setup Instructions

### Prerequisites
- Visual Studio 2022 (or Visual Studio Code with C# extension)
- .NET 8 SDK
- SQL Server Express / LocalDB (comes with Visual Studio)

### Steps

1. **Clone the repository**
   ```bash
   git clone https://github.com/arjundroid12/student-registration-webapp.git
   ```

2. **Open in Visual Studio**
   - Open `StudentRegistrationWebApp.sln` or the `.csproj` file

3. **Set connection string** (already configured in `appsettings.json`)
   ```
   Server=(localdb)\mssqllocaldb;Database=StudentRegistrationWebApp;Trusted_Connection=True;MultipleActiveResultSets=true
   ```

4. **Apply migrations & create database**
   - Open Package Manager Console: Tools → NuGet Package Manager → Package Manager Console
   - Run: `Update-Database`
   - This creates all Identity tables + application tables + seeds roles, admin, and sample courses

5. **Run the application**
   - Press F5 or Ctrl+F5

### Default Admin Account
- **Email:** admin@studentapp.com
- **Password:** Admin@123

### Database Seeding
On first run, the app automatically:
- Creates "Admin" and "Student" roles
- Creates the Administrator account
- Seeds 5 sample courses (CS101, CS201, CS301, CS202, CS401)

## Project Structure

```
StudentRegistrationWebApp/
├── Controllers/
│   ├── HomeController.cs          — Home page (anonymous)
│   ├── AccountController.cs       — Login, Register, Logout, AccessDenied
│   ├── CoursesController.cs       — Course CRUD (Admin), View (all)
│   ├── StudentsController.cs      — Student list (Admin only)
│   ├── ProfileController.cs       — Student profile view/edit
│   └── CourseRegistrationController.cs — Course registration (Student)
├── Data/
│   ├── ApplicationDbContext.cs    — EF Core DbContext + Identity
│   └── DbInitializer.cs          — Seed roles, admin, courses
├── Models/
│   ├── ApplicationUser.cs         — Extended IdentityUser
│   ├── Course.cs                  — Course entity
│   ├── CourseRegistration.cs      — Student-Course mapping (1:1)
│   └── ErrorViewModel.cs
├── Views/
│   ├── Shared/_Layout.cshtml      — Role-based navigation menu
│   ├── Home/Index.cshtml          — Welcome page
│   ├── Account/                   — Login, Register, AccessDenied
│   ├── Courses/                   — Index, Details, Create, Edit, Delete
│   ├── Students/Index.cshtml      — Admin: student list
│   ├── Profile/                   — Index, Edit
│   └── CourseRegistration/        — Register for course
├── Program.cs                     — Identity + DI + middleware
├── appsettings.json               — Connection string
└── StudentRegistrationWebApp.csproj
```

## Authorization Attributes Summary

| Controller | Action | Role Required |
|---|---|---|
| Home | Index | Anonymous (all) |
| Courses | Index, Details | Anonymous (all) |
| Courses | Create, Edit, Delete | Admin |
| Students | Index | Admin |
| Profile | Index, Edit | Student |
| CourseRegistration | Register, Drop | Student |
| Account | Login, Register, AccessDenied | Anonymous |
| Account | Logout | Authenticated |

## Technologies
- ASP.NET Core 8 MVC
- Entity Framework Core 8
- ASP.NET Core Identity
- SQL Server (LocalDB)
- Bootstrap 5
