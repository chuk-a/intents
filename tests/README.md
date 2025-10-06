# Test Files

Test files contain example user input sentences and the expected intent that should be recognized. These files are used to validate that sentence templates correctly match user input.

See [the website](https://developers.home-assistant.io/docs/voice/intent-recognition/test-syntax/) for more detailed information about test syntax.

## Directory Structure

Test files should be placed in `tests/<language>/<domain>_<intent>.yaml`. For example:

- `tests/en/fan_HassTurnOn.yaml` - English test sentences for turning on fans
- `tests/en/light_HassTurnOff.yaml` - English test sentences for turning off lights
- `tests/fr/climate_HassClimateSetTemperature.yaml` - French test sentences for setting temperature

Each language directory must also contain:
- `_fixtures.yaml` - Defines test areas and entities used in tests

## File Format

Test files follow this YAML structure:

```yaml
language: "<language code>"
tests:
  - sentences:
      - "turn on the fan in the kitchen"
      - "turn on all fans in the kitchen"
      - "turn on the kitchen fan"
    intent:
      name: "<intent name>"
      slots:
        area: "kitchen"
        domain: "fan"
        name: "all"
```

### Required Fields

- `language` - The language code (e.g., "en", "fr", "de")
- `tests` - List of test cases
  - `sentences` - List of example sentences that should match
  - `intent` - The expected intent to be recognized
    - `name` - The intent name (e.g., "HassTurnOn", "HassTurnOff")
    - `slots` - Optional dictionary of expected slot values

### Fixtures File Format

The `_fixtures.yaml` file defines test data:

```yaml
language: "<language code>"
areas:
  - name: "Kitchen"
    id: "kitchen"
  - name: "Living Room"
    id: "living_room"
entities:
  - name: "Bedroom Lamp"
    id: "light.bedroom_lamp"
    area: "bedroom"
  - name: "Kitchen Switch"
    id: "switch.kitchen"
    area: "kitchen"
```

## Naming Convention

Test files must match the naming pattern: `<domain>_<intent>.yaml`

The domain should match the domain in the filename, unless the intent's default domain differs. For example:
- `fan_HassTurnOn.yaml` tests the `HassTurnOn` intent for the fan domain
- `homeassistant_HassTurnOn.yaml` tests the `HassTurnOn` intent for all domains

## Contributing Test Files

When contributing:

1. Place test files in the appropriate `tests/<language>/` directory
2. Name the file to match the corresponding sentence file: `<domain>_<intent>.yaml`
3. Include at least one test sentence for every sentence template pattern
4. Run tests with: `pytest tests --language <language> -k <domain>_<intent>`
5. Validate with: `python3 -m script.intentfest validate --language <language>`
