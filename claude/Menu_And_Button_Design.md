**Title: Menu and Button Design for Touch and Non-Touch Interfaces**

**Exam Question:** "Discuss menu and button design in touch interfaces. What constraints govern sizing, placement and menu structure, and how would the same design have to be reworked for a non-touch interface driven by a mouse, a keyboard or a remote control?"

**Answer:**
The starting point for this question is that **buttons and menus are the same abstraction under two completely different input devices**, and almost everything that distinguishes good touch design from good desktop design follows from the properties of the pointer. A mouse cursor is one pixel wide, perfectly precise, visible at all times, and it can *hover* over a target without committing to it. A finger is blunt, imprecise, physically occludes the target it is pressing, and has no state between "not touching" and "pressed". Design rules that were correct for thirty years of desktop interfaces become wrong the moment the pointer changes.

### Buttons in touch interfaces

The sizing rule is physical, and the examiner will expect the numbers. A human thumb ranges from a minimum of about **8 millimetres for a child to a maximum of 18 millimetres for an adult**, so the **minimum usable widget size is 7 millimetres**, raised to **9 millimetres on large tablets**. Proximity is a second, independent constraint: two 7-millimetre buttons must be separated by **at least 2 millimetres**. The general relationship is a trade-off — if controls must sit close together they have to be larger, and if they must stay small they have to be spread further apart. The number of errors rises directly as button size falls, and a Google study from 2013 found that **83% of websites present interaction buttons too small to be used with a finger**, which is exactly the failure mode of porting a desktop layout unchanged.

There is also a rule about *when* to enlarge a control beyond the minimum: increase its dimensions if recovering from a mistaken press would cost the user **more than two interactions, more than five seconds, or a context switch**. This is a cost-of-error argument rather than an aesthetic one, and it is worth quoting because it shows the sizing rules are derived, not arbitrary.

Placement then follows the **comfort zone**, and the millimetre is in fact a poor unit for interface work, since the actual minimum depends on where on the screen the button sits. Two further rules complete the picture. First, **do not crowd the interface** — fewer widgets, more content, more gestures — because every unnecessary control invites a **garbage tap**, an interaction that adds no information and completes no task, as opposed to a **quality tap** which does. Second, because a touchscreen is virtual and gives no mechanical travel, the button must supply **artificial feedback**: haptic vibration, or a visual cue such as the key briefly enlarging when pressed.

### Menus in touch interfaces

Menus inherit all the button constraints and add a structural one. The guiding principle is the **just-in-time interface**: show only what is needed at that moment. Main operations must be available and selectable from a list; secondary information is reached through **progressive disclosure**, where the primary information is immediately visible and further detail costs one more interaction. Context menus should anticipate the user's needs, and the conventional ordering of entries is **opening/sharing actions first, then inserting actions, then mutating actions** — with the destructive ones last, for the same safety reason that keeps "delete account" outside the thumb zone.

Long menus are actively harmful on touch. Because there is **no tab key**, every field and every entry costs an additional tap and interrupts the flow. The practical guidance is therefore: prefer a **list of buttons to a menu when the list is short**, avoid **long drop-down menus** in favour of a dataset, and for numeric input show an average value with **+/− buttons** rather than making the user type.

The distinctive touch alternative is the **circle menu** (pie or radial menu), which exists largely to reduce conflicts between gestures. Its advantages are that it is **easy to learn through muscle memory and fast to use** once learned, since each option lives at a fixed direction from the centre rather than at a position that must be read. The practical capacity is **up to 8 items on a smartphone before precision becomes a problem, up to 18 on a tablet**, and menus of this kind can be **nested**. It suits primary navigation, context menus, tool palettes and games — Microsoft OneNote's radial menu is the standard example, where each sector holds either an item or a further menu. The disadvantages are equally examinable: circle menus **require more precision**, are **not scalable** (on a phone, realistically three or four options), are **not easy to use the first time**, and **cannot change over time** — because their entire benefit is muscle memory, and muscle memory is destroyed by a layout that moves.

### The same design for non-touch interfaces

Reworking this for a mouse, keyboard or remote is not a matter of scaling pixels; four things change.

**Precision changes, so sizing relaxes.** With a one-pixel cursor the 7-millimetre floor disappears, and dense toolbars, small icons and tightly packed dropdown menus become perfectly usable. Fitts's-law reasoning still applies — larger and closer targets are still faster — but it is now an optimisation rather than a hard usability limit. Screen corners and edges become *especially* valuable, because the cursor stops there, whereas on touch they are the least comfortable regions.

**Occlusion disappears, so placement inverts.** The "content always on top" rule exists because a hand covers the screen. A mouse cursor covers nothing, so controls can sit *above* content — which is precisely why desktop applications have always put menu bars and toolbars along the top edge. The tablet rule that a control must never sit above the content it governs is a touch rule, and it does not transfer.

**Hover comes back, and it is the big one.** The hover state is the desktop's mechanism for previewing, for tooltips, and for cascading submenus that open on pass-over. **On touch interfaces the hover event does not exist**, apart from proprietary stylus solutions like the Surface pen or the Samsung Stylus. The standard workaround is to make **the first tap act as the hover and the second as the real click** — which doubles the interaction cost — or, on the web, to use the CSS3 `:hover` property for a more appropriate solution. Designing *for* non-touch therefore means you may rely on hover for progressive disclosure; designing *from* non-touch *to* touch means every hover-dependent affordance has to be redesigned. This is the single most common source of broken touch ports.

**Navigation may become sequential rather than spatial.** A keyboard or a TV remote does not point at all; it moves a **focus** from one control to the next with Tab or a directional pad. This changes menu structure fundamentally. A radial menu, whose whole advantage is that every option lies in a distinct *direction*, becomes nearly meaningless when the user cannot point — the classic empirical comparison of **pie versus linear menus** (Callahan, Hopkins, Weiser and Shneiderman) measured pie menus as faster for a *mouse*, precisely because selection is directional. Under sequential focus navigation, a **linear menu is the correct structure**, because the interaction is "move down N times, confirm". Consequently a non-touch design needs an explicit and visible **focus indicator**, a sensible tab order, and shallow menus — since every extra level costs a full traversal.

Two things do carry across unchanged, and saying so makes for a strong closing remark. **Feedback is always required** — it is merely supplied differently: a mechanical key gives it physically, a mouse button gives a click, and a touchscreen must synthesise it with haptics or animation. And **grouping related controls** to reduce travel is beneficial for a thumb, a cursor and a d-pad alike, because in every case it is the distance between successive targets that costs the user time.

**Key Concepts:**

- **7 mm minimum / 9 mm on tablets / 2 mm separation:** The physical sizing and spacing rules for touch targets.
- **Thumb range 8–18 mm:** The anthropometric basis for those rules.
- **Cost-of-error rule:** Enlarge a control if a mistake costs >2 interactions, >5 seconds, or a context switch.
- **Garbage tap vs quality tap:** An interaction that adds nothing versus one that adds information, completes a task, or adds a smile.
- **Just-in-time interface / progressive disclosure:** Show only what is needed now; detail on demand.
- **Circle (pie/radial) menu:** Muscle-memory-based, up to 8 items on phone / 18 on tablet, nestable; but imprecise, unscalable and must never be rearranged.
- **The missing hover:** Absent on touch; worked around by "first tap = hover, second tap = click".
- **Focus navigation:** Sequential Tab/d-pad traversal on keyboard and remote interfaces, favouring linear menus.
- **Pie vs Linear Menus (Callahan et al.):** The empirical study behind directional menu selection.

**Example/Application:**
A media application offering *play, pause, subtitles, audio track, quality, cast*. On a **phone**, six items exceed what a radial menu handles comfortably, so the two primary actions become large on-screen buttons inside the thumb zone and the remaining four collapse into a sheet opened by a floating trigger button — with subtitles and quality revealed by progressive disclosure rather than shown up front. On the **TV version**, the same six items become a single **linear row** traversed with the remote's d-pad, with a bold focus highlight, tooltips that appear on focus (the remote's equivalent of hover), and no size constraint at all — but with the ordering now critical, since reaching the sixth item costs five button presses.

**Exam Focus:**
The professor is checking that you know the design rules are **consequences of the input device**, not conventions. Give the **numbers** — 7 mm, 9 mm, 2 mm, 8–18 mm, 83% of websites — because these are the details that earn marks. For the non-touch half, the highest-value points are the **absence of the hover event and the first-tap/second-tap workaround**, the **inversion of "content always on top"** once occlusion disappears, and the shift from spatial pointing to **sequential focus navigation**, which is what makes linear menus right for remotes and radial menus right for mice. Be ready to give both the advantages *and* the four disadvantages of circle menus — students routinely list only the advantages.
