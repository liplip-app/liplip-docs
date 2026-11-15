LipLip conceptual ERD — Child Profile Only (revision 2)
Six pages: module map, full overview, four detailed modules.
The full overview omits attributes by design; see detailed modules. PNGs are rendered from the SVGs at 2x resolution.

Revision 2 changes
- Scene is now the weak entity STORY_SCENE (owner STORY, partial key SceneNo, attributes SceneText and SceneImage), linked by the identifying relationship HAS_SCENE (STORY 1 : N STORY_SCENE, total participation of STORY_SCENE).
- HAS_CHECKPOINT (STORY M : N CONTENT_ITEM, attribute CheckpointOrder) is replaced by CHECKPOINT (STORY_SCENE N : 1 CONTENT_ITEM, both partial). A scene has at most one checkpoint word, and the same word can be a checkpoint in several scenes of one story (for example, the bell word in scenes 2 and 5 of "LipLip and the lost bell"). Scene order gives the checkpoint order.
- Entity count is now 18 (M2 Clinical content: 5 entities).
- Page 01: two overlapping cardinality labels were moved (SUPERVISES "N" at CHILD_PROFILE, TRIGGERS "1" at ATTEMPT).
- Changed files: 00_Module_map, 01_Full_conceptual_ERD, 03_Clinical_content (SVG + PNG) and LipLip_ERD.drawio. Pages 02, 04 and 05 are unchanged.
