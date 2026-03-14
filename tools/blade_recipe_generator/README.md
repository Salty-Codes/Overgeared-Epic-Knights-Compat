# Blade Recipe Generator

Automates creation of all required recipe JSON files for a new blade/tool type
under `src/main/resources/data/epicoverknights/recipes/**`.

## Quick start

```sh
# Preview what would be created (no files written)
./tools/blade_recipe_generator/generate_blade_type.sh --type saber --dry-run

# Generate all files
./tools/blade_recipe_generator/generate_blade_type.sh --type saber

# Overwrite already existing files
./tools/blade_recipe_generator/generate_blade_type.sh --type saber --force
```

## What it generates

| Folder                          | Files                                              |
|---------------------------------|----------------------------------------------------|
| `recipes/forging/<type>_blade/` | one per metal material                             |
| `recipes/casting/<type>_blade/` | three per material (blasting / furnace / smelting) |
| `recipes/knapping/`             | `stone_<type>_blade.json` (can be disabled)        |
| `recipes/crafting/<type>/`      | one per material (blade + hilt → weapon)           |

## Flags

| Flag                      | Default | Description                                      |
|---------------------------|---------|--------------------------------------------------|
| `--type <name>`           | —       | **Required.** New blade type name (e.g. `saber`) |
| `--dry-run`               | off     | Print planned operations without writing files   |
| `--force`                 | off     | Overwrite existing files                         |
| `--with-knapping`         | **on**  | Generate stone knapping recipe                   |
| `--without-knapping`      | off     | Skip stone knapping recipe                       |
| `--with-stone-forging`    | off     | Generate stone forging recipe                    |
| `--without-stone-forging` | **on**  | Skip stone forging recipe                        |

## Customisation

### Forging / casting patterns

Edit the template files in `templates/`:

| File                        | Controls                           |
|-----------------------------|------------------------------------|
| `forging.json.tpl`          | Forging recipe structure & pattern |
| `casting_blasting.json.tpl` | Blast-casting recipe               |
| `casting_furnace.json.tpl`  | Furnace-casting recipe             |
| `casting_smelting.json.tpl` | Smelting-casting recipe            |
| `knapping_stone.json.tpl`   | Stone knapping recipe              |
| `crafting.json.tpl`         | Shapeless crafting (blade + hilt)  |

Available placeholders in every template:

| Placeholder             | Replaced with                            |
|-------------------------|------------------------------------------|
| `__TYPE__`              | blade type (e.g. `saber`)                |
| `__MATERIAL__`          | material name (e.g. `iron`)              |
| `__TIER__`              | forging tier (e.g. `iron`, `stone`)      |
| `__EXPERIENCE__`        | casting experience value (e.g. `0.2`)    |
| `__FORGING_KEY_KIND__`  | `"item"` or `"tag"`                      |
| `__FORGING_KEY_VALUE__` | item/tag id (e.g. `forge:ingots/copper`) |

### Materials & balancing

Edit `config/materials.tsv` (tab-separated):

```
# material  tier    experience  key_kind  key_value                      forge  cast  craft
iron        iron    0.2         item      overgeared:heated_iron_ingot   1      1     1
gold        iron    0.25        tag       forge:ingots/gold              1      1     1
```

- Set `forge_enabled`, `casting_enabled`, `crafting_enabled` to `0` to skip a category for a specific material.
- Lines starting with `#` are ignored.

