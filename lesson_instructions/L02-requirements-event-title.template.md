# Requirements — Rename event title (L-02)

**Student:** Brachi leabovitz   **Date:** 22/10/2026  **Fork URL:** https://github.com/BrachiLeabovitz/event-photo-capstone

## 1. Original request (do not reinterpret)
> I entered the wrong event name. I want to be able to change the event name displayed in the client's gallery.

## 2. Current behavior — verified facts
| Claim | Evidence: file/function/line or HTTP result | Verified? |
| --- | --- | --- |
| Event title exists in data model | לכל אירוע יש שדה `title` מסוג `string` שמייצג את שם האירוע. מקור: `src/domain/types.ts` – בתוך `EventRecord` | כן |
| Current client gallery presents title | שם האירוע מוצג בגלריה של הלקוחה באמצעות הפונקציה `getClientGallery`, שמחזירה את `event.title`. מקור: `src/services/event-service.ts` | כן |
| No rename endpoint exists in inspected route file | בקוד הקיים אין אפשרות לשנות את שם האירוע, מכיוון שלא מוגדרים נתיב או פונקציה לעדכון השם. מקור: `src/http/app.ts` ו־`src/services/event-service.ts` | כן |
מה משפיע ומה מושפע בקוד עם התעסקות עם titel?



## 3. Actors / users / use cases / permissions
- Verified: הצלמת מנהלת את האירועים והלקוחה רואה את הגלריה.
- Must clarify: מי רשאי לשנות את שם האירוע – הצלמת בלבד או גם הלקוחה.

## 4. Goal, scope, out of scope
- Goal: לתקן את שם האירוע שמוצג בגלריה של הלקוחה.
- In scope (provisional): שינוי שם האירוע בלבד; הצגת השם החדש בגלריה.
- Out of scope (provisional): שינוי פרטים אחרים של האירוע; שינוי התמונות באירוע.
- Undecided:
  - האם אפשר לשנות שם גם אחרי פרסום הגלריה.
  - האם השם החדש צריך להתעדכן גם ברשימת האירועים.
  - מהו האורך המותר של השם, והאם יש תווים אסורים.

## 5. Functional requirements
- FR-1 (זמני): לאפשר למשתמשת מורשית לשנות את שם האירוע.
- FR-2 (זמני): להציג את השם החדש בגלריה.
- FR-3 (זמני): לא לשנות את התמונות באירוע.

## 6. Non-functional requirements / constraints
- NFR-1: השינוי צריך להתבצע בזמן סביר (לא הוגדר כמה זמן – לא סגור).
- NFR-2: רק משתמשת מורשית תוכל לשנות את השם.
- Constraint: כרגע המערכת שומרת נתונים בזיכרון ואין אפשרות לשנות שם אירוע.

## 7. Ten analysis lenses from the preparation assignment
| Term | Question relevant to this change | Answer known? / evidence |
| --- | --- | --- |
| functional requirements | האם המערכת צריכה לאפשר שינוי שם אירוע גם לאחר פרסום הגלריה? | לא ידוע |
| non-functional requirements | תוך כמה זמן השם החדש צריך להופיע בגלריה לאחר השינוי? | לא ידוע |
| actors / users | מי רשאי לשנות את שם האירוע, הצלמת בלבד או גם הלקוחה? | לא ידוע |
| use cases | מה צריך לקרות מרגע שמשתמשת מבקשת לשנות שם ועד שהשם החדש מוצג בגלריה? | לא ידוע |
| constraints | האם יש מגבלות קיימות על שינוי שם של אירוע שפורסם? | לא ידוע |
| assumptions | האם אנחנו מניחים שרק הצלמת רשאית לשנות את שם האירוע? | הנחה שעדיין לא אושרה |
| acceptance criteria | כיצד נבדוק ששינוי השם הצליח ושהגלריה מציגה את השם החדש? | ראו סעיף 9 (מותנה) |
| ambiguity | האם הבקשה מתייחסת לשם המוצג רק בגלריית הלקוחה או גם ברשימת האירועים? | לא ידוע |
| missing information | מהו האורך המרבי המותר לשם האירוע, והאם יש תווים אסורים? | לא ידוע |
| scope | האם המשימה כוללת רק שינוי שם האירוע או גם שינוי פרטים נוספים שלו? | רק שינוי שם האירוע (ראו סעיף 4) |

## 8. Questions, assumptions, ambiguities
| Question or assumption | Why it matters | Evidence/status | Who decides? |
| --- | --- | --- | --- |
| מי רשאי לשנות את שם האירוע – הצלמת בלבד או גם הלקוחה? | | לא ידוע | לא נקבע |
| האם אפשר לשנות שם אירוע גם לאחר פרסום הגלריה? | | לא ידוע | לא נקבע |
| האם השם החדש צריך להתעדכן גם ברשימת האירועים וגם בגלריית הלקוחה? | | לא ידוע | לא נקבע |
| האם קיימות הגבלות על אורך שם האירוע או על התווים שמותר להזין? | | לא ידוע | לא נקבע |
| מה המערכת צריכה לעשות אם מנסים לשנות שם של אירוע שלא קיים? | | לא ידוע | לא נקבע |
| הנחה: רק הצלמת יכולה לשנות את השם | | עדיין לא אושר | לא נקבע |

## 9. Acceptance criteria (provisional if decisions unresolved)
- Given משתמשת מורשית, when היא משנה את שם האירוע, then השם החדש יופיע בגלריה.
- Given משתמשת לא מורשית, when היא מנסה לשנות את שם האירוע, then המערכת לא תאפשר את השינוי.

## 10. Error cases and tests
- Unknown event: לא הוגדר מה המערכת צריכה לעשות (שאלה פתוחה).
- Unauthorized requester: המערכת לא תאפשר את השינוי.
- Empty title: לא נבדק עדיין.
- Existing client gallery after change: הגלריה מציגה את השם החדש והתמונות באירוע לא משתנות.

## 11. AI checkpoints: understanding / questioning / spec review
Record one claim you verified in source and at least two AI suggestions with adopted/rejected/open status.

Verified claim: בקוד הקיים אין אפשרות לשנות את שם האירוע – לא מוגדרים נתיב או פונקציה לעדכון השם (`src/http/app.ts`, `src/services/event-service.ts`).

### AI question and spec review suggestions — reviewed critically
| AI suggestion | Essential / later / irrelevant / unverified | Justification | Evidence |
| --- | --- | --- | --- |
| לבדוק מי יכול לשנות את שם האירוע (התקבלה) | Essential | חשוב לדעת למי מותר לשנות את השם. | |
| להוסיף כפתור לשינוי שם האירוע (נדחתה) | Later | כרגע אנחנו רק כותבים את הדרישות, ולא מחליטים איך המסך ייראה. | |

## 12. Remaining decisions (do not silently invent answers)
- מי יכולה לשנות את השם.
- האם אפשר לשנות שם אחרי פרסום הגלריה.
- מהו האורך המותר של השם, והאם יש תווים אסורים.
- האם השם החדש מתעדכן גם ברשימת האירועים.
- מה קורה כשמנסים לשנות שם של אירוע שלא קיים.

## 13. Personal reflection
- One fact discovered through code review: בקוד הקיים אין נתיב או פונקציה לעדכון שם אירוע, ושם האירוע מוצג בגלריה דרך `getClientGallery` שמחזירה את `event.title`.
- One AI suggestion you rejected or postponed, and why: דחיתי את ההצעה להוסיף כפתור לשינוי שם האירוע, כי כרגע אנחנו רק כותבים את הדרישות ולא מחליטים איך המסך ייראה.
