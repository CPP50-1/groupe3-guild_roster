# Fantasy Guild Roster - Skeleton

Every workshop deliverable is a `raise NotImplementedError("TODO ...")`
instead of working code. Fill them in as you go through Days 1–6.

Fill in the TODOs. Each one is tagged with the day and (where relevant)
the developer it belongs to.

## Running the tests

```bash
cd fantasy_guild_roster_skeleton
pip install pytest
python -m pytest -v
```

Right now, **40 tests fail and 11 pass**, that's the expected starting
point, not a bug. The 11 that pass exercise infrastructure that's already
given (see below). As you fill in TODOs, more should go green.

## What's given vs. what's a TODO

To avoid a chicken-and-egg problem (several later-day mechanisms are
things *earlier* days' code needs merely to run), a few pieces are
provided working from the start rather than blanked:

| File            | Given (working)                                                                                       | TODO                                                                                                                 |
|-----------------|-------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------|
| `exceptions.py` | Everything, including `batch_validation`                                                              | —                                                                                                                    |
| `fields.py`     | `Field`, `Validated`, `StringField`, `IntField`                                                       | `FloatField` (Day 4 deliverable)                                                                                     |
| `items.py`      | `Rarity`, `Item.__init__`                                                                             | All of `Item`'s dunders (Day 1)                                                                                      |
| `models.py`     | `Character.__init__`, subclass `describe_role()`s, `Paladin`'s declaration, `GuildMeta.registry` dict | `Character`'s dunders (Day 1); `HealerMixin`/`TankMixin`/`LoggableMixin` bodies (Day 4); `GuildMeta.__new__` (Day 5) |
| `roster.py`     | Constructors that just store arguments                                                                | Everything else (Day 2)                                                                                              |
| `combat.py`     | `AmbushError`                                                                                         | `battle()` (Day 3)                                                                                                   |
| `quests.py`     | The three static quest-source generators                                                              | Everything itertools-based (Day 3)                                                                                   |
| `dungeon.py`    | —                                                                                                     | `floor_encounters`, `dungeon_floors`, `guild_transaction` (Day 3)                                                    |

**Why `fields.py`'s descriptor mechanism is given rather than blanked:**
`Character` uses `StringField`/`IntField` for its own fields from the very
first line of `models.py`, and `Roster`/`combat`/`quests` all construct
`Character` instances starting Day 2, long before Day 4 descriptors. 
Blanking the mechanism itself would break every earlier
day's tests before Day 4 is even reached. Instead, the mechanism is
demonstrated already-built, and the actual hands-on Day 4
deliverable is `FloatField`, a new field type following the same pattern.

**Why `GuildMeta` is given as an empty shell, and `Character` doesn't use
it yet:** unlike a regular blank method, a metaclass's `__new__` runs at
**class-definition time**, if it were blanked and `Character` already
declared `metaclass=GuildMeta`, the `Character` class itself would fail to
exist at import time, breaking every single test in the project
regardless of which day it belongs to. Instead, `Character` is declared as
a plain class for now, and wiring in the metaclass (once it's implemented)
is explicitly the last step of the Day 5 TODO. Until then,
`test_metaclass_registers_concrete_subclasses` and
`test_metaclass_rejects_non_int_base_hp` are *expected* to fail.

## Per-day focus (which tests to expect green by end of day)

| Day | Focus files                                        | Tests that should go green                                                               |
|-----|----------------------------------------------------|------------------------------------------------------------------------------------------|
| 1   | `items.py`, dunders in `models.py`                 | `test_items.py`, `test_dunder_*` in `test_models.py`                                     |
| 2   | `roster.py`                                        | `test_roster.py`                                                                         |
| 3   | `combat.py`, `quests.py`, `dungeon.py`             | `test_combat.py`, `test_quests.py`, `test_dungeon.py`                                    |
| 4   | Mixins in `models.py`, `FloatField` in `fields.py` | `test_paladin_*`, `test_healer_mixin_*`, `test_loggable_mixin_*`, `test_float_field_*`   |
| 5   | `GuildMeta` + wiring it into `Character`           | `test_metaclass_*`                                                                       |
| 6   | Integration                                        | `test_integration.py`, full suite green (or best-effort per your "partial is fine" call) |

Note Day 1's dunder tests and Day 4/5's mixin/metaclass tests both live in
`test_models.py` — that file won't be fully green until Day 5.
