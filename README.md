# Multi-Agent Observer Feedback

Research data for *Same Behavior, Different Feedback: Interpreting Multi-Agent Coordination Beyond Performance Metrics*. This release contains the 108 agent trajectories viewed in the main study and the requested original responses from 216 human observers.

## Data files

| Path | Unit | Count | Contents |
| --- | --- | ---: | --- |
| `data/trajectory_manifest.csv` | trajectory | 108 | Trajectory IDs, conditions, source filenames, file checksums |
| `data/trajectories/T*.json` | trajectory | 108 | Agent interaction traces with original `staticInfo` and `dynamicState` |
| `data/participants.csv` | participant | 216 | Release-local participant IDs and performance groups |
| `data/episode_ratings.csv` | participant × trajectory | 864 | Overall collaboration rating for each video viewed |
| `data/feedback.jsonl` | feedback item | 1,746 | Feedback text, reason text, tone, and optional video frame interval |
| `data/post_survey.jsonl` | participant | 216 | Four post-study free-text answers |

Each participant viewed four trajectories, and each trajectory was viewed by eight participants. Of the 1,746 feedback items, 1,595 have an explicit frame interval and 151 do not. The field definitions, question wording, rating scales, and missing-value conventions are in [the data dictionary](docs/data_dictionary.md).

## Load and join

```python
import csv
import json

with open("data/episode_ratings.csv", newline="", encoding="utf-8") as f:
    ratings = list(csv.DictReader(f))
with open("data/feedback.jsonl", encoding="utf-8") as f:
    feedback = [json.loads(line) for line in f]

feedback_by_observation = {}
for item in feedback:
    feedback_by_observation.setdefault(item["observation_id"], []).append(item)

first_rating = ratings[0]
print(first_rating["trajectory_id"], len(feedback_by_observation.get(first_rating["observation_id"], [])))
```

Join feedback to ratings by `observation_id`, ratings to participants by `participant_id`, and ratings to the trajectory manifest by `trajectory_id`. The manifest points to the exact agent trace JSON for each video.

## Scope and privacy

The response files were extracted with a field allowlist. Recruitment-platform IDs, source document IDs, dates and exact timestamps, session and playback logs, pilot responses, monitoring-difficulty responses, and analytic code labels are not in this release. Participant and response IDs were generated randomly for this release; no crosswalk to source IDs is distributed. Video frame indices are retained because they locate the behavior described in feedback; they are not clock times.

Written responses are verbatim. Automated screening found no email addresses, phone numbers, URLs, postal addresses, platform-ID references, or exact dates in the released text, but automated screening cannot guarantee that free text contains no identifying details. Verbatim responses may also be matched to copies of the text previously made public outside this repository. Researchers should take care not to attempt to identify participants.

## Provenance and integrity

The 108 selected trajectory JSON files were copied byte-for-byte from the experiment repository at commit `ae59b7e76dd7057a3aae5b66a685cc068edf4956`. `data/trajectory_manifest.csv` records their source filenames and SHA-256 hashes. `SHA256SUMS` lists SHA-256 hashes for every released data file. The release was checked for file integrity, row counts, valid joins, rating and interval ranges, and common identifier patterns.

## License and citation

The dataset is released under [Creative Commons Attribution 4.0 International](LICENSE). When reusing it, credit the dataset and cite the paper when its final publication details are available. A versioned archival DOI will be added after the Zenodo dataset deposit is published.
