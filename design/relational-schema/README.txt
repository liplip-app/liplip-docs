LipLip — Relational Schema (revision 2)

Source: LipLip conceptual ERD, Child Profile Only, revision 2.
24 relations = 18 entities + 2 multivalued-attribute relations + 4 M:N relationship relations.

Files
- Relational_Schema.html: easy-to-read view in the browser, with a Constraints section at the end.
- Relational_Schema.md: the same content in Markdown.
- 00_Full_Relational_Schema and 01–04 module pages (SVG + PNG): print-ready relation lists with explicit FK targets. Page 03 is unchanged.
- LipLip_Relational_Schema.dbml: import into dbdiagram.io. It contains types, NOT NULL, UNIQUE, CHECK and composite FKs (needs a DBML parser with `checks` support).
- LipLip_Schema_SQLServer.sql: SQL Server DDL generated from the DBML, plus the filtered unique index. Parse-checked, not yet run on a SQL Server instance.
- Foreign_Key_Overview.svg / .dot: FK arrows (child table -> referenced table; dashed = nullable FK).

Revision 2 changes
- USER table renamed APP_USER (USER is a reserved word in SQL Server). The ERD entity is still called USER.
- STORY_CHECKPOINT removed. STORY_SCENE (weak entity, PK StoryID + SceneNo) gets ItemID FK? -> CONTENT_ITEM, mapping the N:1 CHECKPOINT relationship. The same word can now appear in more than one scene of a story.
- CONSENT: (ParentID, ChildID) is a composite FK to PARENT_CHILD, so only a guardian linked to the child can give consent.
- ATTEMPT: CHECK that exactly one of SessionID / AssessmentID is set. Manual-mark fields (MarkedOutcome, MarkedByUserID, MarkedAt) are filled together, and OutcomeSource = 'Manual' requires a mark.
- THERAPY_PLAN: filtered unique index (ChildID) WHERE PlanStatus = 'Active', so a child has at most one active plan.
- CHILD_PROFILE: LinkedAt and LinkStatus must be NULL when SpecialistID is NULL.
- ACTIVITY_ITEM and SESSION_ACTIVITY: UNIQUE on the order column within the parent.
- APP_USER.Email UNIQUE. NOT NULL on required columns. Surrogate keys are IDENTITY. Status/enum columns are sized nvarchar(10–20), CONTENT_ITEM.Text is nvarchar(200).
- CHECK value lists for LinkStatus, Gender, ApprovalStatus and PlanStatus are proposals: confirm them with the backend team before the first migration.

Mapping decisions
- USER specialization: APP_USER + ADMIN, SPECIALIST, PARENT subtype tables. Subtype UserID is both PK and FK. UserType is the discriminator (CHECK Admin | Specialist | Parent).
- Composite attributes Name, Recording, APIResult and FaceMeasures are flattened. Permission and GuidanceCue map to separate relations.
- M:N relationships use composite PKs of the two FK columns (CARES_FOR, REVIEWS, USES, INCLUDES).
- 1:N relationships map to FK columns on the N side, nullable for partial participation. SUPERVISES keeps at most one current specialist per child (no link history). CONTENT_REVIEW keeps the latest review per specialist and item (no review history). Both are deliberate.
- PLAN_GOAL and STORY_SCENE are weak entities with composite PKs (PlanID, GoalNo) and (StoryID, SceneNo).
- RuleID and AvatarID are scalar identifiers with no entity in the ERD. AvatarID maps to the avatar_01..avatar_12 asset files.

Rules enforced in the backend (not expressible with plain FKs or CHECKs)
- ATTEMPT.ChildID matches the child of its SESSION's plan or of its ASSESSMENT.
- Every child profile has at least one PARENT_CHILD row (total participation).
- Only a Specialist or the child's Parent can mark an attempt (MarkedByUserID role check).
- A specialist is verified before being linked to children or approving plans.
