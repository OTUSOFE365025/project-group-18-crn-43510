**Constraints Table:**
| **ID**  | **Constraint** |
|----------|----------------|
| **CON-1** | The platform must be used while connected to the platform network (will not operate when user is offline). |
| **CON-2** | The platform must preserve system state after disconnections, which requires backups to a non-volatile memory database. |
| **CON-3** | To log onto the platform, the user must be redirected to the institution's single sign-on (SSO) before accessing the system. |
| **CON-4** | The system’s data must be in sync with the university systems data, which should be checked every 5 seconds. |