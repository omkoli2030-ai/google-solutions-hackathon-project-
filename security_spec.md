# Security Specification - Guardian Hospitality

## Data Invariants
1. A Guest can only create an incident where `userId` is their own UID.
2. Only Staff or Admin can update the `status` of an incident.
3. Once an incident is marked as `resolved`, it cannot be reopened or edited except by an Admin.
4. Messages in an incident subcollection must belong to the incident's participants (reporter or responding staff).
5. Global announcements can only be created by Admins.

## The Dirty Dozen (Attack Vectors)
1. **Identity Theft**: Guest `A` tries to create an incident with `userId: "GuestB"`.
2. **Privilege Escalation**: Guest tries to update their role to `admin` in `/users/UID`.
3. **Ghost Update**: Guest tries to change `status: "resolved"` on their own incident to avoid inspection.
4. **Information Leak**: Guest tries to list ALL incidents in the system.
5. **Orphaned Message**: User tries to post a message to an incident that doesn't exist.
6. **Time Spoof**: User sets `createdAt` to 10 years in the future to mess with logs.
7. **Resource Exhaustion**: Malicious user sends a 1MB string as a `name` during registration.
8. **ID Poisoning**: User tries to create an incident with a document ID of 1000 characters.
9. **Role Injection**: Guest tries to assign themselves a `responderId` context.
10. **Global Hijack**: Guest tries to post a global announcement.
11. **PII Scraping**: Public user tries to read the entire `/users` collection.
12. **Status Shortcutting**: User moves incident from `active` directly to `resolved` without a `responderId` assigned.

## Test Runner (Logic Verification)
I will implement `firestore.rules` to prevent these.
