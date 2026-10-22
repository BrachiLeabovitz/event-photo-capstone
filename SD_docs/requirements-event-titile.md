# Requirements — Rename event title (L-02)

**Student:** Brachi leabovitz   **Date:** 22/10/2026  **Fork URL:** https://github.com/BrachiLeabovitz/event-photo-capstone

## 1. Original request (do not reinterpret)
> I entered the wrong event name. I want to be able to change the event name displayed in the client's gallery.

## 2. Current behavior — verified facts
| Claim | Evidence: file/function/line or HTTP result | Verified? |
| --- | --- | --- |
| Event title exists in data model | `src/domain/types.ts` | Yes |
| Current client gallery presents title | `src/services/event-service.ts` | Yes |
| No rename endpoint exists in inspected route file | `src/http/app.ts` | Yes |
## 3. Actors / users / use cases / permissions
- Verified:
  - Event host / owner can view the event gallery and details.
  - Client / guest can view the event title displayed in the gallery.
- Must clarify:
  - Is anyone with access to the event allowed to rename it, or only the event owner / host?
  - Do we need role-based authentication/authorization (permissions check) for the rename operation?
## 4. Goal, scope, out of scope
- Goal:
  - Allow authorized users to change the event title so the updated title is displayed in the client's gallery.
- In scope (confirmed):
  - Adding/updating an endpoint/service logic to support updating the event title.
  - Updating the title stored in the data store.
  - Displaying the updated title in the gallery view.
- Out of scope (confirmed):
  - Changing other event attributes (e.g., event date, photos, location).
  - Redesigning the gallery UI layout.
- Undecided:
  - Specific authentication/authorization middleware implementation for renaming (pending clarification from section 3).
## 5. Functional requirements
- FR-1: The system shall provide an API endpoint (e.g., PATCH/PUT /events/:id/title) to update an existing event's title.
## 6. Non-functional requirements / constraints
- NFR-1: Response time for updating the event title via the API endpoint should be under 200ms under standard operational load.
- NFR-2: Data consistency: The updated event title must be immediately visible upon subsequent fetch requests.
- NFR-3: Backward compatibility: Existing event query endpoints must continue to return the standard event payload format including the updated title.
## 7. Ten analysis lenses from the preparation assignment
| Term | Question relevant to this change | Answer known? / evidence |
| --- | --- | --- |
| functional requirements | How is the title updated and displayed? | Yes / API endpoint update and gallery fetch (`src/http/app.ts`) |
| non-functional requirements | What are the performance and consistency expectations? | Yes / Immediate persistence and response under 200ms |
| actors / users | Who is viewing and requesting the title change? | Yes / Event host and client gallery viewer |
| use cases | What is the primary user flow? | Yes / Changing an incorrectly entered event title |
| constraints | What technology constraints exist? | Yes / Existing TypeScript codebase and Node HTTP routes |
| assumptions | Is authentication currently enforced for this endpoint? | No / Pending clarification on authorization model |
| acceptance criteria | How do we verify the title change succeeded? | Yes / Given valid title, when PATCH sent, then 200 OK and new title stored |
| ambiguity | Can event titles contain special characters or empty strings? | Partial / Must enforce non-empty string validation |
| missing information | Are there title length limits or role permission requirements? | Yes / Need product clarification on max length & permissions |
| scope | Does this change affect other event attributes or media files? | Yes / Out of scope; limited strictly to event title rename |
## 8. Questions, assumptions, ambiguities
| Question or assumption | Why it matters | Evidence/status | Who decides? |
| --- | --- | --- | --- |
| Who can rename an event? | Determines whether we need authorization checks in the endpoint | Open / Needs product decision | Product Owner / Client |
| Are there minimum/maximum length constraints on the title? | Affects input validation logic and error handling | Assumption: 1–100 characters | Development / Product |
| Should we keep an edit history/audit log for title changes? | Affects database model requirements | Assumption: No audit log needed for now | Product Owner |
## 9. Acceptance criteria (provisional if decisions unresolved)
- Given a valid existing event ID and a non-empty string new title, when a request is made to rename the event, then the system updates the title in the data store and returns a 200 OK status.
- Given an invalid or empty event title, when a request is made to rename the event, then the system returns a 400 Bad Request error and does not update the event title.
- Given a non-existent event ID, when a request is made to rename the event, then the system returns a 404 Not Found error.
- Given an event that was successfully renamed, when a client fetches the event gallery, then the newly updated title is returned and displayed.
## 10. Error cases and tests
- Unknown event:
- Unauthorized requester:
- Empty title:
- Existing client gallery after change:

## 11. AI checkpoints: understanding / questioning / spec review
| AI suggestion | Essential / later / irrelevant / unverified | Justification | Evidence |
| --- | --- | --- | --- |
| Add endpoint to rename event title | Essential | Directly satisfies the primary user request to fix incorrectly entered event names. | `src/http/app.ts` |
| Add role-based permissions check for renaming | Later | Important for security, but core functionality can be implemented first while auth requirements are clarified. | Section 3 & Section 8 |
| Add title change audit log / history table | Irrelevant | Out of scope for a basic event title update requirement. | Section 4 (Scope) |

## 12. Remaining decisions (do not silently invent answers)
- Whether any user can update the event title or if strictly authenticated event owners/hosts are permitted.
- Maximum and minimum character length constraints for event titles (e.g., max 100 characters).

## 13. Personal reflection
- One fact discovered through code review:
  The current server routes in `src/http/app.ts` only handle basic event retrieval and display, lacking any existing update/PATCH endpoints for event properties.
- One AI suggestion you rejected or postponed, and why:
  I postponed implementing full role-based authorization for the rename endpoint because user permissions and authentication mechanisms are not yet defined in the current spec and need product clarification.