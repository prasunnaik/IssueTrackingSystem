Week 3 Training Project — Issue Tracking System

I am working on a Week 3 training case study for an Issue Tracking System.

I already have a Spring Boot backend from a previous training project for this same Issue Tracking System. I do NOT want to rebuild the backend from scratch. Treat the existing backend as the baseline and only suggest backend modifications when the Week 3 requirements genuinely require them.

My main task for Week 3 is to build the Angular frontend and integrate it with the existing Spring Boot microservices.

⸻

1. Project Context

The application is an Issue Tracking System where users have two roles:

* PROJECT_OWNER
* ASSIGNEE

The Project Owner can:

* Login/signup
* Create projects
* View their projects
* View project issues
* Create issues
* Edit issues
* Filter issues
* Search issues

The Assignee can:

* Login
* View assigned issues
* View issue details
* Change issue status
* Save status updates

⸻

2. Existing Backend

Assume the backend was already developed using:

* Java
* Spring Boot
* Spring MVC / REST
* Spring Data JPA / Hibernate
* MySQL
* Microservices
* Eureka Service Discovery
* Spring Cloud API Gateway
* Layered architecture
* Controller → Service → Repository
* Exception handling
* Validation
* REST APIs
* Git/GitHub

The Angular frontend should communicate primarily through the API Gateway, not directly with individual microservices.

Expected architecture:

Angular
↓
API Gateway
↓
User Service / Project Service / Issue Service
↓
MySQL

Do not introduce unnecessary technologies or redesign the backend unless required.

⸻

3. Existing APIs

User Controller

GET /api/v1/users

POST /api/v1/users

POST /api/v1/users/login

GET /api/v1/users/{userId}

GET /api/v1/users/{userId}/issues

GET /api/v1/users/username/{username}/issues

Project Controller

GET /api/v1/projects/{projectId}

PUT /api/v1/projects/{projectId}

DELETE /api/v1/projects/{projectId}

GET /api/v1/projects

POST /api/v1/projects

GET /api/v1/projects/{projectId}/users

GET /api/v1/projects/{projectId}/insights

GET /api/v1/projects/username/{username}/issues

Issue Controller

GET /api/v1/issues/{id}

PUT /api/v1/issues/{id}

GET /api/v1/issues

POST /api/v1/issues

GET /api/v1/issues/project/{projectId}

GET /api/v1/issues/owner/{ownerId}

GET /api/v1/issues/assignee/{assigneeId}

Before writing Angular API calls, ask me for the actual backend code/DTOs if the exact request or response structure is unknown. Never invent API request/response JSON.

⸻

4. Week 3 Case Study Requirements

Milestone 1 — Login and Signup

Login

Login must have:

* Email
* Password
* Role

Requirements:

* Email required
* Valid email format
* Password required
* Role required
* Role options:
    * Project Owner
    * Assignee
* Submit disabled if form invalid
* Signup link navigates to Signup
* Validation messages shown when user interacts with fields
* Use Template-Driven Forms

Signup

Fields:

* Name
* Email
* Password
* Profile image URL
* Role

Requirements:

* Name required
* Email required and valid
* Password required
* Profile image URL required and valid URL
* Role required
* Role options:
    * Project Owner
    * Assignee
* Submit disabled when invalid
* Login link navigates to Login
* Validation messages shown
* Use Reactive Forms

⸻

5. Project Owner Dashboard

Dashboard must contain:

Header

* Project title
* Search bar

Sidebar

* Profile image
* Username
* Email
* Number of projects created
* Number of issues created
* Project Dashboard
* Create Project
* Create Issue
* Logout

Dashboard

Project dropdown should contain projects owned by logged-in Project Owner.

First project returned by API should be selected by default.

Display:

* Project Owner name
* Start date
* End date
* Issues

Issues should appear in:

* TO_DO
* DEVELOPMENT
* TESTING
* COMPLETED

Issue cards should show relevant details such as:

* Title/summary
* Description
* Creation date
* Priority
* Assignee
* Assignee profile photo

Filters:

* Filter by Assignee
* Filter by Priority

If no projects exist:

Display:

No projects Available

and provide a link/button to Create Project.

⸻

6. Create Project

Fields:

* Project Name
* Project Owner
* Project Start Date
* Project End Date

Validation:

Project Name

* Required
* Maximum 150 characters
* No special characters

Project Owner

* Required
* Dropdown populated from API

Dates

* Required
* End date must not be earlier than start date

Use Reactive Forms.

Create button:

* Enabled only when form is valid
* Disabled while API request is in progress
* On success → navigate to Project Dashboard
* On failure → display server error

Reset button:

* Clear all fields

⸻

7. Create Issue

Fields:

* Summary
* Type
* Project
* Description
* Priority
* Assignee
* Tags
* Sprint
* Story Point
* Status

Validation:

Summary

* Required
* Maximum 150 characters
* No special characters
* Case study also mentions minimum 5 characters where applicable

Type

Required.

Mapping:

1 = BUG
2 = TASK
3 = STORY

Project

Required.
Populate from Project API.

Description

Optional according to Create Issue screen.
Maximum 500 characters.

Priority

Required.

Mapping:

1 = LOW
2 = MEDIUM
3 = HIGH

Assignee

Required.
Populate from Users API.

Tags

Optional.
Maximum 100 characters.

Sprint

Positive integer.

Story Point

Positive integer.

Status

Required.

Mapping:

1 = TO_DO
2 = DEVELOPMENT
3 = TESTING
4 = COMPLETED

Use Reactive Forms.

Create button:

* Disabled when invalid
* Disabled while API request is running
* Success → Project Dashboard
* Failure → display server error

Reset → clear form.

⸻

8. Issue Details / Edit Issue

Issue details should display:

* Summary
* Project
* Type
* Priority
* Description
* Assignee
* Sprint
* Story Point
* Status
* Tags
* Created date
* Last updated date

Edit functionality must allow updating the issue.

Edit button:

* Enabled only when required fields are valid
* Disabled while API request is in progress
* Success → dashboard/details page
* Failure → show server error

Reset should restore the form to its initial state.

⸻

9. Assignee Dashboard

Header:

* Application name
* Search bar

Sidebar:

* Profile image
* Username
* Email
* Number of assigned issues
* Logout

Issues should be displayed in:

* TO_DO
* DEVELOPMENT
* TESTING
* COMPLETED

Issue cards should display:

* Title
* Description
* Creation date
* Priority
* Assignee details

Search should match summary/description using Regex as specified in the case study.

⸻

10. Assignee Issue Details

When an Assignee clicks an issue:

Display all relevant issue information.

Status must be selectable from:

* TO_DO
* DEVELOPMENT
* TESTING
* COMPLETED

The “Save Updates” button should:

* Initially be disabled
* Become enabled after status changes
* Call PUT /api/v1/issues/{id}
* Update the issue status
* Handle success/failure appropriately

⸻

11. Angular Technology Requirements

Use:

* Angular
* TypeScript
* HTML
* CSS
* Angular Router
* HttpClient
* RxJS
* Services
* Dependency Injection
* Template-driven forms where explicitly required
* Reactive Forms where explicitly required

Follow Angular best practices.

Avoid:

* any
* unnecessary duplication
* huge components
* hardcoded API responses
* unnecessary libraries
* unnecessary backend changes

Use strongly typed interfaces/models.

⸻

12. Recommended Angular Structure

Use a maintainable structure similar to:

src/app/

core/

* services/
    * auth.service.ts
    * user.service.ts
    * project.service.ts
    * issue.service.ts
* guards/
* interceptors/

models/

* user.model.ts
* project.model.ts
* issue.model.ts

components/

* login/
* signup/
* project-owner-dashboard/
* create-project/
* create-issue/
* issue-details/
* edit-issue/
* assignee-dashboard/
* assignee-issue-details/

shared/

* header/
* sidebar/
* issue-card/

Do not create unnecessary files until they are needed.

⸻

13. Important Development Rule

Build the project milestone by milestone.

Do NOT give me the entire application at once.

Follow this sequence:

1. Angular setup
2. Models
3. Services
4. Routing
5. Login
6. Signup
7. Project Owner Dashboard
8. Create Project
9. Create Issue
10. Issue Details
11. Edit Issue
12. Assignee Dashboard
13. Assignee Issue Details
14. API integration/testing
15. Validation/error handling
16. UI improvements
17. Git/GitHub

After completing each milestone, verify it before moving to the next.

⸻

14. Very Important: Work With My Existing Backend

Whenever I provide backend code, inspect it carefully.

Do not assume DTO fields, endpoint payloads, entity relationships, or response formats.

If you need something from the backend, explicitly tell me:

“Please provide X file/code because I need it to implement Y.”

For example:

“Please provide the Issue DTO because I need to match the Angular Issue model to the backend response.”

Do not invent backend code.

If you identify a backend change, explain:

1. Why it is required
2. Which backend file needs modification
3. What exactly needs to change
4. Whether it could affect existing functionality

Prefer the smallest possible backend modification.

⸻

15. Coding Style

When giving me code:

* Give complete files when practical.
* Clearly mention the file path.
* Don’t give disconnected snippets that cannot work together.
* Keep code beginner-friendly but professional.
* Use proper TypeScript types.
* Explain important parts.
* Don’t over-engineer.
* Follow the case study exactly.
* Preserve existing code unless there is a reason to change it.

When modifying an existing file, tell me exactly what changed.

⸻

16. Debugging Rule

When I give you an error:

1. Identify the likely cause.
2. Ask for the relevant file if necessary.
3. Give the exact correction.
4. Explain why the error happened.
5. Do not randomly change unrelated files.

If the error is caused by the backend, clearly tell me that.

If it is caused by Angular, clearly tell me that.

⸻

17. Technical Interview Preparation

This is extremely important.

I will have a technical interview based on this project.

Whenever we complete a major feature, give me a short section:

Interview Questions

Include likely questions such as:

* Why did you use Reactive Forms here?
* Why Template-Driven Forms for Login?
* Why did you create a service?
* How does Angular call Spring Boot?
* What is HttpClient?
* What is an Observable?
* Why use Dependency Injection?
* How does routing work?
* How do you validate the form?
* How do you handle API errors?
* How does the dashboard filter issues?
* How does the frontend communicate with the API Gateway?
* How is authentication handled?
* Why shouldn’t we use any?
* Explain the component → service → API flow.

Give short, interview-ready answers, not long theoretical explanations.

⸻

18. Teaching Style

I am still learning Angular.

Teach me like a developer who already knows Java/Spring Boot but is learning Angular.

When explaining:

* Relate Angular concepts to Spring Boot concepts when useful.
* Keep explanations simple.
* Tell me what I actually need to know for this project.
* Don’t teach unrelated Angular topics.
* Don’t overwhelm me with theory.

For example:

Angular Service ≈ a reusable service layer for API/business communication.

Angular Component ≈ UI/controller-like responsibility, but don’t claim they are exactly the same.

⸻

19. Interview Mode

If I say:

“interview mode”

Then answer my questions very briefly, as if I am answering an interviewer.

Do not give huge explanations unless I ask for them.

If I ask:

“What is a service?”

Answer in 2–4 sentences.

If I ask:

“Explain this code.”

Explain only the relevant code.

⸻

20. Final Goal

Help me build a working Week 3 Issue Tracking System that:

* Matches the provided case study
* Uses our existing Spring Boot backend
* Uses Angular correctly
* Has proper validation
* Has clean architecture
* Has API integration
* Has good error handling
* Is GitHub-ready
* Can be demonstrated to the trainer
* Can be explained confidently in the technical interview

Start by asking me to provide the existing backend project/code or the relevant backend files if you need to verify the API contracts.

Then begin with Step 1: Angular project setup and proceed one milestone at a time.
