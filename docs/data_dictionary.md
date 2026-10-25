# Data dictionary

## Relationship and identifiers

`participant_id`, `observation_id`, and `feedback_id` are newly generated random pseudonyms. They are stable **within this candidate export** and have no embedded source identifier or timestamp. Rebuilding the export generates different IDs. `trajectory_id` is a release-local identifier; `source_filename` and `sha256` in the manifest trace each trajectory to the selected source file.

## `trajectory_manifest.csv` — 108 trajectories

| Field | Meaning |
| --- | --- |
| `trajectory_id` | Release-local trajectory key, `T001`–`T108` |
| `file` | Relative path to the copied trajectory JSON |
| `source_filename` | Relative filename in the experiment's `src/maps` directory |
| `layout_name` | Factory layout recorded in the trajectory's `staticInfo` |
| `team_size` | Number of agents, 2 or 4 |
| `performance_group` | `Low`, `Mid`, or `High` assigned during stimulus selection |
| `state_count` | Number of objects in `dynamicState`; 201 in this release |
| `final_score` | Final cumulative delivery score in `dynamicState` |
| `sha256` | SHA-256 checksum of the copied trajectory JSON |

Each trajectory JSON retains its original `staticInfo` and `dynamicState` structure. The study videos used the first 200 environment steps; the stored array includes the initial state.

## `participants.csv` — 216 participants

| Field | Meaning |
| --- | --- |
| `participant_id` | Random release-local participant key |
| `performance_group` | Assigned `Low`, `Mid`, or `High` stimulus group |

## `episode_ratings.csv` — 864 observations

| Field | Meaning |
| --- | --- |
| `observation_id` | Random release-local key for one participant viewing one trajectory |
| `participant_id` | Foreign key to `participants.csv` |
| `trajectory_id` | Foreign key to `trajectory_manifest.csv` |
| `episode_order` | Viewing order, 1–4; not a clock time |
| `collaboration_rating` | Five-point overall collaboration rating; 1 = Poor, 5 = Excellent |

## `feedback.jsonl` — 1,746 feedback items

| Field | Meaning |
| --- | --- |
| `feedback_id` | Random release-local feedback-item key |
| `observation_id` | Foreign key to `episode_ratings.csv` |
| `start_frame`, `end_frame` | Inclusive video frame indices, 0–199; both `null` if the observer could not specify a range |
| `tone` | Participant's five-point self-rating of feedback tone; 1 = Negative, 5 = Positive |
| `feedback_text` | Verbatim answer to “What feedback would you like to give?” |
| `reason_text` | Verbatim answer to “What is the reason for this feedback?”; can be `null` |

## `post_survey.jsonl` — 216 participants

| Field | Question |
| --- | --- |
| `participant_id` | Foreign key to `participants.csv` |
| `q1` | “Overall, what was your strategy for monitoring the AI robot's behavior?” |
| `q2` | “Across all episodes, what challenges did you experience while monitoring the AI agents?” |
| `q3` | “What, if anything, would have helped you monitor the AI agents more easily?” |
| `q4` | “Final message for the AI robots you watched across all the episodes?” |

The survey answers are verbatim. `q1`–`q4` are participant-level responses and should not be duplicated into each observation when analyzing independent units.
