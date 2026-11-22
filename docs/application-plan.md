# Application Plan

## Pages

| Page                     | Description                                                                                               |
| :----------------------- | :-------------------------------------------------------------------------------------------------------- |
| Landing                  | Short introduction to TemuTim with Register and Login buttons                                             |
| Register                 | Form to create an account with a `ui.ac.id` email                                                         |
| Login                    | Form to sign in                                                                                           |
| My Team                  | Dashboard with the posts I created, the posts I applied to or joined, request statuses, and team contacts |
| Browse Posts             | List of open team posts with keyword search and filters                                                   |
| Post Detail              | Full information for one team post, the join button, and the request list for the owner                   |
| Create Post              | Form for a new team post                                                                                  |
| Edit Post                | Form to edit or close a team post, for the owner only                                                     |
| Profile                  | A student's profile, visible to any logged-in student                                                     |
| Edit Profile             | Form to edit my own profile                                                                               |
| Find Students (optional) | List of student profiles filtered by faculty and skill                                                    |

## Access

Every page except Landing, Register, and Login requires login. Edit Post and Edit Profile are limited to the owner. A visitor who is not logged in is sent to Login.

## Shared Layout

Every page extends `base.html` and uses the same header:

- Site name TemuTim
- Links to My Team, Browse Posts, Create Post, and my Profile
- Login and Register links when logged out, and a Logout control when logged in

Flash messages for success and error notices appear below the header.

## Landing

The page explains the idea in a few sentences: students post a competition they need teammates for, and students from any faculty can apply. It has Register and Login buttons.

## Register and Login

Register collects:

| Field      | Control                               |
| :--------- | :------------------------------------ |
| Name\*     | Text input                            |
| Email\*    | Email input, must end with `ui.ac.id` |
| Password\* | Password input, stored as a hash      |

Login collects email and password. Both forms show a validation message when a field is empty or wrong, and the Register form rejects an email that is already used.

## My Team

The page has three parts:

- Posts I created, each with the requests waiting for my decision
- Posts I applied to or joined, each with my request status (pending, accepted, or rejected)
- Team contacts for every post where I am a member

Contact details appear only after a request is accepted.

## Browse Posts

The page lists open posts. Each card shows the competition name, organizer, category, deadline, remaining slots, and wanted skills, with a link to the full post.

Search and filters cover:

- Keyword
- Category
- Faculty
- Skill
- Deadline not yet passed

## Post Detail

The page shows:

- Competition name and organizer
- Category and description
- Info link and deadline
- Number of members needed and remaining slots
- Wanted skills and wanted faculties
- Status (open or closed) and the owner, linked to their profile

What a visitor can do depends on who they are:

| Viewer                      | Controls                                                                  |
| :-------------------------- | :------------------------------------------------------------------------ |
| Student who has not applied | Join form with a short message and a Send Request button                  |
| Student who has applied     | Status of their own request, with no second join form                     |
| Owner                       | Request list with Accept and Reject buttons, plus Edit and Close controls |
| Team member                 | Contacts of the other members                                             |

The join form is hidden when the post is closed. The owner never sees it.

## Create Post and Edit Post

Edit Post uses the same form, filled with the current values, and adds a Close Post button. Fields marked with an asterisk are required.

| Field              | Control                  |
| :----------------- | :----------------------- |
| Competition name\* | Text input               |
| Organizer          | Text input               |
| Category\*         | Dropdown (fixed list)    |
| Description\*      | Text area                |
| Info link          | URL input                |
| Deadline\*         | Date picker              |
| Members needed\*   | Number input, at least 1 |
| Wanted skills      | Multi-select             |
| Wanted faculties   | Multi-select             |

The form shows a validation message when a required field is empty. It ends with Submit and Cancel buttons. A closed post cannot be reopened by editing its slots.

## Profile and Edit Profile

The profile page shows name, faculty, study program, class year, bio, skills, and competition experience. The contact is hidden unless the viewer is in the same team. Only the owner sees an Edit Profile button.

| Field                  | Control                                   |
| :--------------------- | :---------------------------------------- |
| Name\*                 | Text input                                |
| Faculty\*              | Dropdown                                  |
| Study program\*        | Text input                                |
| Class year\*           | Number input                              |
| Bio                    | Text area                                 |
| Skills                 | Multi-select, several allowed             |
| Competition experience | Text area                                 |
| Contact\*              | Text input, for example a WhatsApp number |

## Find Students (Optional)

The page lists student profiles with filters for faculty and skill. Each row links to the profile.

## Skill Match Score (Optional)

On Post Detail, the page compares the viewer's skills with the post's wanted skills and shows how many match. The score uses set intersection.

## Auto-close After Deadline (Optional)

A post whose deadline has passed is shown as closed, and its join form is hidden.

## Form Handling

Every form uses Flask-WTF, which adds validation and CSRF protection. A rejected form keeps what the user typed and shows the message next to the field.

