# Full Gitmoji reference

The complete official gitmoji list (from the canonical `gitmojis.json`), with a suggested Conventional Commits type for each. Use the emoji character itself in the commit. The `:shortcode:` is given for reference — GitHub/GitLab render shortcodes into the emoji, but most teams (and this skill) use the literal emoji so it shows up in plain `git log` too.

When a change maps to several of these, pick the one matching the *dominant intent*, and prefer the more specific entry.

| Emoji | Shortcode | Meaning | Suggested type |
|---|---|---|---|
| 🎨 | `:art:` | Improve structure / format of the code | refactor |
| ⚡️ | `:zap:` | Improve performance | perf |
| 🔥 | `:fire:` | Remove code or files | refactor |
| 🐛 | `:bug:` | Fix a bug | fix |
| 🚑️ | `:ambulance:` | Critical hotfix | fix |
| ✨ | `:sparkles:` | Introduce new features | feat |
| 📝 | `:memo:` | Add or update documentation | docs |
| 🚀 | `:rocket:` | Deploy stuff | chore |
| 💄 | `:lipstick:` | Add or update the UI and style files | style |
| 🎉 | `:tada:` | Begin a project | chore |
| ✅ | `:white_check_mark:` | Add, update, or pass tests | test |
| 🔒️ | `:lock:` | Fix security or privacy issues | fix |
| 🔐 | `:closed_lock_with_key:` | Add or update secrets | chore |
| 🔖 | `:bookmark:` | Release / version tags | chore |
| 🚨 | `:rotating_light:` | Fix compiler / linter warnings | style |
| 🚧 | `:construction:` | Work in progress | chore |
| 💚 | `:green_heart:` | Fix CI build | ci |
| ⬇️ | `:arrow_down:` | Downgrade dependencies | build |
| ⬆️ | `:arrow_up:` | Upgrade dependencies | build |
| 📌 | `:pushpin:` | Pin dependencies to specific versions | build |
| 👷 | `:construction_worker:` | Add or update CI build system | ci |
| 📈 | `:chart_with_upwards_trend:` | Add or update analytics or track code | feat |
| ♻️ | `:recycle:` | Refactor code | refactor |
| ➕ | `:heavy_plus_sign:` | Add a dependency | build |
| ➖ | `:heavy_minus_sign:` | Remove a dependency | build |
| 🔧 | `:wrench:` | Add or update configuration files | chore |
| 🔨 | `:hammer:` | Add or update development scripts | chore |
| 🌐 | `:globe_with_meridians:` | Internationalization and localization | feat |
| ✏️ | `:pencil2:` | Fix typos | fix |
| 💩 | `:poop:` | Write bad code that needs to be improved | refactor |
| ⏪️ | `:rewind:` | Revert changes | revert |
| 🔀 | `:twisted_rightwards_arrows:` | Merge branches | — (merge) |
| 📦️ | `:package:` | Add or update compiled files or packages | build |
| 👽️ | `:alien:` | Update code due to external API changes | fix |
| 🚚 | `:truck:` | Move or rename resources (files, paths, routes) | refactor |
| 📄 | `:page_facing_up:` | Add or update license | chore |
| 💥 | `:boom:` | Introduce breaking changes | feat!/fix! |
| 🍱 | `:bento:` | Add or update assets | chore |
| ♿️ | `:wheelchair:` | Improve accessibility | feat |
| 💡 | `:bulb:` | Add or update comments in source code | docs |
| 🍻 | `:beers:` | Write code drunkenly | chore |
| 💬 | `:speech_balloon:` | Add or update text and literals | feat |
| 🗃️ | `:card_file_box:` | Perform database related changes | feat |
| 🔊 | `:loud_sound:` | Add or update logs | chore |
| 🔇 | `:mute:` | Remove logs | chore |
| 👥 | `:busts_in_silhouette:` | Add or update contributor(s) | docs |
| 🚸 | `:children_crossing:` | Improve user experience / usability | feat |
| 🏗️ | `:building_construction:` | Make architectural changes | refactor |
| 📱 | `:iphone:` | Work on responsive design | style |
| 🤡 | `:clown_face:` | Mock things | test |
| 🥚 | `:egg:` | Add or update an easter egg | feat |
| 🙈 | `:see_no_evil:` | Add or update a .gitignore file | chore |
| 📸 | `:camera_flash:` | Add or update snapshots | test |
| ⚗️ | `:alembic:` | Perform experiments | chore |
| 🔍️ | `:mag:` | Improve SEO | feat |
| 🏷️ | `:label:` | Add or update types | feat |
| 🌱 | `:seedling:` | Add or update seed files | chore |
| 🚩 | `:triangular_flag_on_post:` | Add, update, or remove feature flags | feat |
| 🥅 | `:goal_net:` | Catch errors | fix |
| 💫 | `:dizzy:` | Add or update animations and transitions | style |
| 🗑️ | `:wastebasket:` | Deprecate code that needs cleanup | refactor |
| 🛂 | `:passport_control:` | Authorization, roles and permissions | feat |
| 🩹 | `:adhesive_bandage:` | Simple fix for a non-critical issue | fix |
| 🧐 | `:monocle_face:` | Data exploration / inspection | chore |
| ⚰️ | `:coffin:` | Remove dead code | refactor |
| 🧪 | `:test_tube:` | Add a failing test | test |
| 👔 | `:necktie:` | Add or update business logic | feat |
| 🩺 | `:stethoscope:` | Add or update healthcheck | chore |
| 🧱 | `:bricks:` | Infrastructure related changes | chore |
| 🧑‍💻 | `:technologist:` | Improve developer experience | chore |
| 💸 | `:money_with_wings:` | Add sponsorships or money infra | chore |
| 🧵 | `:thread:` | Multithreading or concurrency | feat |
| 🦺 | `:safety_vest:` | Add or update validation | feat |
| ✈️ | `:airplane:` | Improve offline support | feat |
| 🦖 | `:t-rex:` | Add backwards compatibility | feat |

The "suggested type" column is guidance, not law — the conventional type should reflect what the change *does* for users of the codebase. When in doubt, `feat` is for new capabilities, `fix` for corrected behavior, and `chore` for everything that doesn't change src behavior.
