# UX and UI audit checklist

The core covers interface behavior, visual implementation, and accessibility. Add modules based on the product's features and platforms. Each result applies to one check on one target under a defined set of conditions. See [SKILL.md](../SKILL.md) for audit execution and [report-format.md](report-format.md) for reporting.

## Contents

- [Visual clarity and information design](visual-content.md)
- [How to read the checklist](#how-to-read-the-checklist)
- [Core](#core)
  - [Goals and navigation](#goals-and-navigation)
  - [Status, errors, and control](#status-errors-and-control)
  - [Visual implementation](#visual-implementation)
  - [Accessibility](#accessibility)
- [Feature and platform modules](#feature-and-platform-modules)
  - [Forms and sign-in](#forms-and-sign-in)
  - [Search, lists, and data visualization](#search-lists-and-data-visualization)
  - [Payments, consent, and permissions](#payments-consent-and-permissions)
  - [Network, timing, and localization](#network-timing-and-localization)
  - [Platforms and input methods](#platforms-and-input-methods)
  - [AI interfaces](#ai-interfaces)
- [Testing with the target audience](#testing-with-the-target-audience)

## How to read the checklist

Use **Applies when** to identify targets within the audit scope. An applicable safeguard that is missing fails the check. For example, UX-12 applies to irreversible deletion even when the product has no confirmation step. Use `not_checked` when evidence or access is missing, and use `not_applicable` only when you have confirmed that the applicability condition is absent.

**Run** means perform the action in the interface. **Measure** means use a tool to obtain a numeric result. **Compare** means check the result against a reference, the accessibility tree, or the observed state. **Research** means use observations from representatives of the target audience. Where a check names more than one method, collect evidence for each.

Set the test order, thresholds, and exceptions before testing. If requirements conflict, record both with their sources and use the priority agreed in advance. Use `not_checked` if that priority is unknown and would change the verdict.

When a check cites WCAG, use the definitions and exceptions in the cited criterion. A check marked as covering one aspect tests only that aspect. The accessibility section covers selected checks. A WCAG conformance claim requires an audit of all applicable criteria and complete processes. [WCAG 2.2 conformance](https://www.w3.org/TR/WCAG22/#conformance), [W3C ACT Rules](https://www.w3.org/WAI/standards-guidelines/act/rules/about/), [W3C WAI Evaluation](https://www.w3.org/WAI/test-evaluate/)

## Core

### Goals and navigation

| ID · Check | Applies when | Test | Yes when |
| --- | --- | --- | --- |
| UX-01 · The flow reaches its goal | Each primary or critical flow | Run. Start from the specified entry point as the specified role. Complete the flow through the interface. | The observed result matches the expected result. For saving, sending, or paying, also verify the result at its destination or in the transaction history. |
| UX-02 · Navigation identifies the current section | At least two sibling sections are visible in the navigation at the same time, and one is open | Run. Open each section in turn. | The current-section indicator matches the open section at every step. |
| UX-03 · The navigation label matches its destination | A link or menu item identifies its destination or purpose | Run and compare. Activate the item and compare its label and intended destination with the actual result. | The interface opens the stated destination or performs the stated action. The heading, content, or result confirms the destination or purpose. |
| UX-04 · Back returns to the previous location | The interface or platform offers Back for this transition | Run. Navigate from A to B and go back. | The interface returns to A or to a return point documented in advance for this transition. |
| UX-05 · The same entity keeps the same name | The same entity or operation appears at different steps in the same language | Compare. Record the terms and check them against the glossary if one exists. | The interface uses one term or an approved glossary variant. Grammatical forms and inflections are allowed. |
| UX-06 · The action label describes its effect | A standalone button, command, or text response changes state or starts a process | Compare and run. Check the label against its effect. If the label answers a dialog question, read them together. | The label names the effect or unambiguously answers a visible question that names it. “OK” is acceptable for dismissing an informational message if dismissal causes no other changes. |
| UX-07 · Prerequisites are visible before the flow starts | A flow requires a known resource or access outside the current interface, such as a document, a code, or a separate role | Run. Open the entry point with the resource missing. | Before the first required step, the interface lists missing resources or shows the exact condition blocking the start. |
| UX-08 · A list preserves its position | A user can open an item from a list and return | Run. Scroll the list, open a visible item, and return. If the list is unchanged, look for the opened item. | On return, the item is visible without scrolling again, or focus returns to it and the list scrolls it into view. |

The core criteria are checklist rules based on these principles. [ISO 9241-11](https://www.iso.org/standard/63500.html), [ISO 9241-110](https://www.iso.org/standard/75258.html), [NN/g Usability Heuristics](https://www.nngroup.com/articles/ten-usability-heuristics/)

### Status, errors, and control

| ID · Check | Applies when | Test | Yes when |
| --- | --- | --- | --- |
| UX-09 · The interface indicates a pending operation | An operation can be kept pending, for example by delaying a service response | Run. Start the operation, hold the response, and check the state at the agreed time. If no response budget is set, hold the response for 3 seconds and check after 1 second. | At the check time, a pending indicator tied to the action or result area is visible or announced. |
| UX-10 · The interface shows the operation outcome | An operation ends in a state change or a final error | Run. Trigger success and a controlled failure separately. | For each outcome, the interface shows the corresponding result, status, or changed value. On a failed run, it gives no success indication. |
| UX-11 · The action can be canceled before confirmation | The action requires confirmation or a separate intermediate screen before it takes effect | Run. Enter the intermediate state, then use Cancel or the standard exit. | The user can cancel, and the intended effect does not occur. |
| UX-12 · The interface protects against irreversible actions | Deletion cannot be undone, publication is irreversible, or a bulk change has no standard rollback | Run. Initiate the action and proceed up to the point where it would take effect. | Before the action takes effect, a separate confirmation step identifies the action and shows the item identifier or the number of items. |
| UX-13 · The draft is protected when the user leaves | Leaving a screen through normal navigation can discard unsaved changes | Run. Enter a unique value, leave, and return. | The interface restores the value, or before leaving it offers the choice to stay or leave and makes clear that leaving discards the changes. |
| UX-14 · Repeated activation produces a single result | An action creates a record, order, or submission where unintended duplicates are prohibited | Run. Activate the action twice before the first response. Count the actual results. | A successful operation produces exactly one result. |
| UX-15 · The user can recover from a temporary failure | A temporary error can interrupt a flow, after which the operation can resume or be retried | Run. Trigger the error, resolve its cause, and take the indicated recovery action. | The recovery action available from the error state produces the expected result once the cause is resolved. |
| UX-16 · An error message identifies what failed | An operation or part of a screen encounters an error | Run. Trigger the error and read or listen to the message. | The message and its placement identify the failed operation or area. The message includes a description beyond any technical code. |
| UX-17 · The empty state is distinct from loading | A screen or section displays a collection that can successfully return zero items | Run. Obtain a successful empty response. | After loading finishes, the interface reports that there are no items through separate text or an announced status. |
| UX-18 · The empty state gives users a way to get items | From an empty screen, the current role can normally create or import an item or clear a filter | Run. Trigger the empty state and take the offered action. | The empty state offers a working route to create or import items or to clear the filter limiting the results. |
| UX-19 · The interface explains why a command is unavailable | A command is visible but unavailable because of prerequisites, permissions, or state | Run. Use a supported input method to find the reason. | The user can find the reason without activating the command. If the user can meet the prerequisite, the interface gives a specific next step. |
| UX-20 · Returning to a previous step preserves input | A multistep flow lets the user go back and edit an earlier step | Run. Enter a unique value at step A, proceed to B, and return to A with the standard control. | The value at A is preserved. Evaluate fields that must be cleared under a requirement recorded in advance against that requirement. |

In UX-09, the check after 1 second and the 3-second response delay are checklist defaults for a reproducible run.

### Visual implementation

Use [visual clarity and information design](visual-content.md) alongside these rendering checks when the user needs to understand or scan a page, whether or not it is interactive.

| ID · Check | Applies when | Test | Yes when |
| --- | --- | --- | --- |
| UI-01 · Full text remains available | Headings, labels, messages, and data are visible at the sizes and in the themes and languages under test | Compare. Check visible content against the full strings, including long test values. | The full string is visible or can be opened, expanded, or scrolled using the input method under test. |
| UI-02 · Actions stay reachable when content overlaps | A screen has scrolling, panels, floating elements, or an on-screen keyboard | Run. At each size under test, trigger the relevant state and perform the action. | The button or field is available in the active layer, either directly or after normal scrolling or dismissal of the overlapping layer. |
| UI-03 · Adjacent targets have distinct active areas | Adjacent elements can be activated by pointer or touch | Measure and run. Determine the active areas in the same state and compare their boundaries. | The active areas of different available actions in the same layer have no area in common. Count a composite component with a single action as one target. |
| UI-04 · The component matches its specified variant | An approved specification exists for the component in this state | Compare. Check the variant, tokens, and dimensions against the reference, accounting for any specified tolerances. | Every property specified for the instance matches the reference or falls within its tolerance. |
| UI-05 · Styling shows which action is primary | The reference specifies a primary action and its styling for a group of actions | Compare. Check the style variant of every action in the group. | The primary action uses the designated primary variant. Every lower-priority action in the group uses a different variant. |
| UI-06 · Headings match the approved hierarchy | A screen with nested sections has defined heading levels and visual styles | Compare. Check headings and their styles against the specified levels. | Each heading uses the style for its level across the states and sizes covered by the reference. |
| UI-07 · The selection indicator matches the data | A list, tab group, switch, or mode selector has a selected state | Run. Change the selection and check the actual data, the visible indicator, and the accessibility tree if the platform exposes it. | All available ways of reading the state report the same selection. |
| UI-08 · Functional assets load | A screen uses icons or images to convey data or identify actions | Run. Open the screen with an empty cache and a working network connection. | Each asset renders or the intended fallback conveys the same information or identifies the same action. |

For UI-04 through UI-06, determine whether a reference exists independently of whether you can access it. If a specification exists and you lack access to it, use `not_checked`. Use RES-04 to test the target audience's perception of a visual property.

### Accessibility

For web interfaces, criterion numbers refer to [WCAG 2.2](https://www.w3.org/TR/WCAG22/). For native and other software, choose a suitable equivalent for each applicable criterion in advance. Use the platform's requirements and the adaptations from [WCAG2ICT](https://www.w3.org/TR/wcag2ict/) where appropriate. Numeric thresholds labeled as web thresholds apply to the web.

| ID · Check | Applies when | Test | Yes when |
| --- | --- | --- | --- |
| AX-01 · Text meets the contrast threshold | Visible text or an image of text falls within WCAG 1.4.3 or its equivalent | Measure. Measure the contrast ratio against the actual background in each applicable theme and state. | On the web, contrast is at least 4.5:1 for regular text and 3:1 for large-scale text. Large-scale text is at least 18 pt or at least 14 pt bold, subject to the WCAG definition for the script. Apply the exceptions in WCAG 1.4.3. |
| AX-02 · Graphics and controls meet the contrast threshold | Boundaries, parts of controls, states, or graphics are needed for identification or reading under WCAG 1.4.11 or its equivalent | Measure. Measure the meaningful element against the adjacent color in each applicable state. | On the web, contrast is at least 3:1, subject to the scope and exceptions of WCAG 1.4.11. |
| AX-03 · Meaning is distinguishable without color | Color conveys a status, action, relationship, or data | Compare. Find a visual cue for each meaning that does not rely on color. | Text, shape, pattern, or another cue lets users distinguish the meanings without identifying colors. See WCAG 1.4.1. |
| AX-04 · The name, role, and value are accessible | An interface with an accessibility API contains an interactive component | Compare and run. Inspect the accessibility tree and check how the state changes during interaction. | The name identifies the purpose, the role matches the behavior, and exposed states and values match the interface and update when it changes. The component meets WCAG 4.1.2 or the platform equivalent. |
| AX-05 · The visible label is part of the accessible name | An interactive element has a visible text label | Compare. Compare the label with the accessible name. | The accessible name contains the visible label text under WCAG 2.5.3 or its equivalent. |
| AX-06 · Keyboard users can operate the interface | A web interface or another interface supports keyboard input | Run. Operate the features under test without pointer input or touch. | The features work with a keyboard, subject to the exception in WCAG 2.1.1 or its equivalent for functions that require the path of movement as part of the input. |
| AX-07 · Keyboard focus can leave the component | A component can receive keyboard focus | Run. Use the keyboard to enter and leave the component. For a modal dialog, check that focus can leave after the dialog closes. | A keyboard user can move focus away. If it takes a nonstandard method, instructions are available. See WCAG 2.1.2. |
| AX-08 · Keyboard focus is visible | An element receives focus during keyboard navigation | Run. Move focus to the element and capture the screen. | A visible indicator identifies the focused element. See WCAG 2.4.7. |
| AX-09 · Focus order preserves the working sequence | Sequential navigation order affects meaning or operation | Run. Record the focus order and follow the steps in that order. | The focus order preserves the intended sequence of interaction and meaning. For example, focus stays out of the background while a modal dialog is open. See WCAG 2.4.3. |
| AX-10 · Status messages reach screen readers without moving focus | The interface shows a status message as defined by WCAG 4.1.3 or a platform equivalent | Run with a screen reader. Trigger the status while keeping focus on the current element. | Assistive technology receives the message without moving focus to it. |
| AX-11 · Non-text content has an alternative | The interface contains an image, chart, or other non-text item, including a decorative item | Compare. Examine the item without relying on visual information and compare the result with the source data. | An alternative conveys the item's meaning or purpose, and assistive technology skips purely decorative items. Apply the special cases in WCAG 1.1.1. |
| AX-12 · Resizing text preserves functionality | A web page contains text | Run. Enlarge text to 200% without assistive technology and perform the applicable actions. | Text and functionality remain available under WCAG 1.4.4 and its exceptions. |
| AX-13 · Reflow preserves content | A web page has content that users read by scrolling vertically or horizontally | Run. Set the width to 320 CSS px for vertical reading or the height to 256 CSS px for horizontal reading. | Information and functionality remain available without scrolling along both axes. Apply the exception in WCAG 1.4.10 for content that requires a two-dimensional layout. |
| AX-14 · Target size or spacing meets the minimum | A web element can be activated with a pointer | Measure. Measure the active area in CSS px. For a smaller target, check the conditions for an exception. | The area is at least 24 × 24 CSS px or a specific WCAG 2.5.8 exception applies. For the spacing exception, check the circles with a diameter of 24 CSS px. |
| AX-15 · Dragging has an alternative | A web feature uses dragging | Run. Use a single pointer to get the same result without dragging, for example by selecting an item and then its destination. | The user can achieve the result without dragging, or an exception in WCAG 2.5.7 applies. |
| AX-16 · Captions convey prerecorded speech | Prerecorded synchronized media contains audio covered by WCAG 1.2.2 | Run and compare. Compare the audio with the captions, including meaningful sounds. | Captions are synchronized and convey the speech and meaningful sounds. Apply the WCAG 1.2.2 exception for clearly labeled media alternatives for text. |
| AX-17 · Supported larger text preserves functionality | A native app uses system text scaling | Run. Set the text to the largest size defined by the platform or product test profile. | Users can read the text and perform the actions. Scrolling and reflow are acceptable. |
| AX-18 · Users can stop moving content | Web content starts moving, blinking, or scrolling automatically, lasts more than five seconds, and appears alongside other content | Run. Check whether the user can pause, stop, or hide the movement. | At least one of these controls is available or the movement is essential to the activity under WCAG 2.2.2. In a full audit, test auto-updating content without motion under the separate provision of the same criterion. |
| AX-19 · A modal dialog manages focus | A web interface contains a modal dialog | Run. Open the dialog, navigate with Tab and Shift+Tab, and close it with Escape. | Focus enters the dialog, stays within it during navigation, and returns to the invoking element when the dialog closes. If that element is gone or the workflow calls for a different focus target, focus moves to the next appropriate element under the APG rules. |
| AX-20 · The page declares its language | A web page contains text | Compare. Check the programmatically set language against the page's primary language. | The page's primary language is correctly determined programmatically under WCAG 3.1.1. |
| AX-21 · Assistive technology can identify headings | Visual headings mark the structure of a web page or native screen | Compare. Check the visible structure against the accessibility tree. | The tree exposes each structural heading as a heading and reports the appropriate level if the platform supports levels. This checks one aspect of WCAG 1.3.1. |
| AX-22 · Users can bypass repeated blocks | A block appears before the main content on multiple web pages | Run. Use a keyboard or supported assistive technology to try to bypass the block. | A working mechanism bypasses the repeated block under WCAG 2.4.1. |
| AX-23 · Focused components stay at least partly visible | A web component receives keyboard focus and the screen contains an overlay or fixed panel | Run. Move focus to the component with the keyboard in the overlapping states under test. | The component is at least partly visible or the WCAG 2.4.11 provision for user-opened content applies. |
| AX-24 · Text spacing changes preserve content | Web text uses a language and script to which the WCAG 1.4.12 properties apply | Run. At the same time, set line height to 1.5 times the font size, spacing after paragraphs to 2 times, letter spacing to 0.12 times, and word spacing to 0.16 times. Keep other properties unchanged. | All text remains available and the interface remains functional with the applicable settings. Apply the language and script exceptions in WCAG 1.4.12. |
| AX-25 · A screen reader user can complete the flow | The screen reader test profile includes a primary or critical flow | Run. Complete the flow with the specified screen reader and its commands, without reading visual information. | All required actions and data are accessible, and the observed result matches the goal of the flow. |

Additional accessibility references. [W3C APG Modal Dialog](https://www.w3.org/WAI/ARIA/apg/patterns/dialog-modal/), [Apple HIG Accessibility](https://developer.apple.com/design/human-interface-guidelines/accessibility/), [W3C Understanding Target Size](https://www.w3.org/WAI/WCAG22/Understanding/target-size-minimum)

## Feature and platform modules

### Forms and sign-in

| ID · Check | Applies when | Test | Yes when |
| --- | --- | --- | --- |
| FORM-01 · A field stays labeled during input | A field requires user input | Run. Enter a value and leave focus in the field. | A visible label continues to identify the field while the user enters a value. |
| FORM-02 · Users can identify required fields before submitting | A form contains required fields | Compare. Open the form before submission and inspect the labels and instructions. | Users can determine which fields are required before submitting, either through markers or an explicit rule for the entire form. |
| FORM-03 · The interface states format restrictions | A field only accepts values in a particular unit, format, or range | Compare. Before entering a value, check the guidance against the format the field accepts. | The interface states the requirement before input. If it shows an example, the system accepts that example in the corresponding test state. |
| FORM-04 · A field error identifies its field | A submission can return an error for a specific field | Run. Submit the form with an invalid value and check the screen and the programmatic association with the field if the platform supports it. | A message next to each invalid field or a direct link from an error summary identifies it. On the web, assistive technology can also access the error. |
| FORM-05 · The error explains how to fix it | The system rejects a field format or value and knows how the user can correct it | Run. Trigger the error and correct the field as the message directs. | The guidance describes an acceptable change, and making that change resolves the error. Apply the exceptions in WCAG 3.3.3 when evaluating that criterion. |
| FORM-06 · Other inputs survive a validation error | A form contains at least two fields | Run. Fill in the form, make one field invalid, and submit. | Values in the other fields persist except where a requirement recorded in advance calls for clearing them. |
| FORM-07 · The flow avoids asking users to reenter information | The user has already entered or received the information in the same process | Run. Proceed to the point that asks for the same information again. | The interface fills in the value automatically or makes it available to select, or an exception in WCAG 3.3.7 or a documented native-software equivalent applies. |
| FORM-08 · Web authentication supports accessible options | A web authentication step requires a cognitive function test, including memorization or transcription | Run. Check for an alternative or support for an assisting mechanism, such as pasting a code or using a password manager. | Each such step meets one of the options allowed by WCAG 3.3.8. |
| FORM-09 · The original destination is available after sign-in | Navigating to a protected resource or step triggers sign-in | Run. Open the resource without a session and sign in with the required permissions. | Sign-in opens the original resource or offers a direct route to it. |
| FORM-10 · Invalid values are rejected | The specification disallows a field value in a particular form state | Run. Submit a predefined invalid example and check the resulting state through the interface or an authorized test data source. | The system leaves the invalid value unsaved and gives no indication of a successful submission. |

Form references. [GOV.UK Error Message](https://design-system.service.gov.uk/components/error-message/), [GOV.UK Error Summary](https://design-system.service.gov.uk/components/error-summary/), [WCAG 2.2 Input Assistance](https://www.w3.org/TR/WCAG22/#input-assistance)

### Search, lists, and data visualization

| ID · Check | Applies when | Test | Yes when |
| --- | --- | --- | --- |
| DATA-01 · Exact search finds a known item | Search accepts an exact key, name, or identifier | Run. With permission to view the item, search by its exact value for a previously created, indexable item. | The item appears within the search window defined by the test scenario. |
| DATA-02 · The interface shows active filters | A user applies a filter that changes the result set | Run. Apply and clear each filter selected for the audit. | The interface shows the current filter, and the observed result set matches the expected result set for the test data. |
| DATA-03 · An empty search keeps the query | A search returns no results | Run. Enter a query with no matches and wait for the result. | The user can edit the query from the zero-results state. |
| DATA-04 · Sorting matches the selected order | A sort control is available | Run. Use test data with varied values and change the sort direction. | The order matches the selected field, direction, and specified rules for tied or empty values. |
| DATA-05 · A bulk action shows the scope of selection | An action applies to selected records, including selections across multiple pages | Run. Select some records, then select all results if that mode is available. | Before executing the action, users can see the number of affected records or a description that identifies the exact set. |
| DATA-06 · Users can relate chart values to the data | A chart shows a numerical measure, time, or multiple series | Compare. Check scales, units, series, and values against a known dataset. | For each displayed value, users can identify the series, category, or period and the unit of measure when the data has one. |
| DATA-07 · Returning from an item preserves list settings | Users can open an item from a filtered or sorted list and return through normal navigation | Run. Apply a filter and sort order, open an item, and return. | The original filter and sort order are still active, unless the transition was identified as a reset. |

### Payments, consent, and permissions

| ID · Check | Applies when | Test | Yes when |
| --- | --- | --- | --- |
| TRUST-01 · The full price is visible before commitment | An action creates a payment obligation | Run. Proceed to the final action and compare the total with a test invoice. | Before confirmation, users see the currency and total with mandatory fees included. The total matches the invoice for the same conditions. |
| TRUST-02 · Auto-renewal terms are visible before subscription | A subscription or trial automatically renews for a fee | Compare. Inspect the screen immediately before signup. | Before confirmation, the screen shows the charge amount and frequency, along with the date of the first charge or a rule for calculating it. |
| TRUST-03 · An optional paid extra requires an active choice | A step offers a paid extra outside the package the user selected | Run. Open the screen in a clean state and proceed without selecting the extra. | The extra starts unselected and stays out of the order. |
| TRUST-04 · Users can cancel a subscription through an available route | An automatically renewing subscription can be managed in the product or platform store | Run. Start from subscription management for a test account and follow the available cancellation route. | The route ends with confirmation that auto-renewal is off or an equivalent store status. |
| TRUST-05 · A system permission request has a visible reason | The product asks for access to the camera, microphone, location, or another protected resource | Run. On a fresh installation, reach the request and compare it with the user action that led to it. | Before the user responds, the interface identifies which feature uses the permission and why. Text in the system prompt can satisfy this check. |
| TRUST-06 · Denying an optional permission preserves an independent flow | Only some features require the permission | Run. Deny the request and complete a flow that does not require the permission. | The user completes the independent flow without seeing the permission request again. |
| TRUST-07 · Users can decline optional consent | A screen asks for consent to an action identified as optional for the main service | Run. Start without stored consent, decline or proceed without making a choice, and complete the main flow. | The interface records the decline or absence of consent and lets the user complete the main flow. |

These checks evaluate the interface. Determine legal requirements separately under the applicable rules. [FTC Bringing Dark Patterns to Light](https://www.ftc.gov/reports/bringing-dark-patterns-light), [Apple HIG Privacy](https://developer.apple.com/design/human-interface-guidelines/privacy/)

### Network, timing, and localization

| ID · Check | Applies when | Test | Yes when |
| --- | --- | --- | --- |
| ENV-01 · Offline save status reflects what happened | An action requires a server or can enter a sync queue | Run. Disconnect the network before performing the action. | The interface distinguishes local saves, queued changes, and server-confirmed saves, or explicitly reports the failure. |
| ENV-02 · A queued change syncs once | The product promises to sync changes made offline | Run. Make one change offline and restore the connection. | Within the sync budget set for the test, the server receives exactly one change and the interface shows its final status. |
| ENV-03 · Formats match the language and settings | The product supports multiple locales or time zones and displays a date, time, number, or currency | Compare. Check known values, including values around midnight when time zones apply. | For the specified locale and time zone, the output matches the agreed format and source value. The currency matches the data unless the product specifies conversion. |
| ENV-04 · Localizable interface text is translated | The locale has an approved glossary or string catalog | Compare. Walk through the flows in this locale, including errors and empty states. | Every localizable string matches the approved text. Product names and other exceptions listed in advance are allowed. |
| ENV-05 · RTL order matches the reference | The product supports a language written from right to left | Compare. Open the screen in right-to-left (RTL) mode and inspect the direction of text and components, including text mixed with numbers. | The order matches the platform or product reference. Values and labels retain their meaning. |
| ENV-06 · Web loading meets the LCP threshold | A web page is intended for end users | Measure. Obtain 75th-percentile Largest Contentful Paint (LCP) from field data for the agreed segment. | p75 LCP ≤ 2.5 seconds. |
| ENV-07 · Web responsiveness meets the INP threshold | A web page has user interactions | Measure. Obtain 75th-percentile Interaction to Next Paint (INP) from field data for the agreed segment. | p75 INP ≤ 200 ms. |
| ENV-08 · Web layout shifts meet the CLS threshold | A web page is intended for end users | Measure. Obtain 75th-percentile Cumulative Layout Shift (CLS) from field data for the agreed segment. | p75 CLS ≤ 0.1. |
| ENV-09 · An operation meets its time budget | An operation has a time budget and defined measurement conditions | Measure. Run the operation several times using the specified procedure. | The agreed metric stays within budget in each required segment. |
| ENV-10 · The interface indicates session expiry | A session can expire during a flow, and users must sign in to continue | Run. End the test session partway through the flow and try to continue. | The interface tells the user to sign in again and provides a way to do so. |

For ENV-06 through ENV-08, record the field data source, period, aggregation level, device, and amount of available data. Evaluate mobile and desktop separately when they are in scope. Use `not_checked` when field data for the required segment is unavailable. Record lab results separately as diagnostics. The thresholds follow Google's web benchmarks. [Google Web Vitals](https://web.dev/articles/vitals)

### Platforms and input methods

| ID · Check | Applies when | Test | Yes when |
| --- | --- | --- | --- |
| PLAT-01 · iOS and iPadOS touch targets meet the recommended size | The test profile includes a native touch target on iOS or iPadOS | Measure. Measure the active area, including invisible padding. | The active area is at least 44 × 44 pt. This profile uses Apple's recommended size as its quality target. |
| PLAT-02 · Android touch targets meet the recommended size | The test profile includes a native touch target on Android | Measure. Measure the active area in dp. | The active area is at least 48 × 48 dp, following Android's recommendation. |
| PLAT-03 · Touch users can complete the flow without hover | The product supports touch and some controls appear on hover on desktop | Run. Complete the same flow using touch only. | All required actions and data are available by touch or through another enabled touch equivalent without a pointer. |
| PLAT-04 · System UI leaves primary actions reachable | A mobile interface has system bars, a display cutout, or a keyboard | Run. Check supported orientations and keyboard states. | Users can reach the required action and read its label in the current state or after normal scrolling or dismissing the keyboard. |
| PLAT-05 · The intended input method supports the entire flow | A TV, kiosk, terminal, voice interface, or other product uses a dedicated input method | Run. Use only the specified remote, physical buttons, voice, switch, or other input method. | The user can complete every step and reach the observable result with that input method. |
| PLAT-06 · Users can recover from an unrecognized command | A voice or text conversation recognizes a limited set of commands | Run. Provide input that the channel accepts but the dialog cannot recognize, then provide corrected input. | The dialog reports that it did not recognize the input and accepts the correction within the same flow. |
| PLAT-07 · The CLI returns the expected exit code | A command can succeed or fail | Run. Reproduce a documented success and failure and capture the process exit codes. | Each code matches the command contract. Under the standard contract, success returns 0 and failure returns a nonzero value. |
| PLAT-08 · Machine-readable CLI output can be parsed | A command promises JSON, CSV, or another machine-readable format | Run. Invoke that output mode and pass stdout to a parser for the stated format. | The parser accepts all of stdout without manually removing any lines. |

Platform references. [Apple HIG Accessibility](https://developer.apple.com/design/human-interface-guidelines/accessibility/), [Android Touch Target Size](https://support.google.com/accessibility/android/answer/7101858), [Command Line Interface Guidelines](https://clig.dev/)

### AI interfaces

| ID · Check | Applies when | Test | Yes when |
| --- | --- | --- | --- |
| AI-01 · The interface explains what the AI can do | A user opens an AI feature for the first time | Compare. Check the description at the entry point or in help accessible from it against the approved list of supported actions. | The description lists the actions in the test profile and makes no promise about actions the profile identifies as unsupported. |
| AI-02 · Users see limitations before acting on AI output | The AI profile defines a type of error that could change the user's action, such as fabricated links or incorrect classification | Run. Trigger the relevant scenario and inspect the interface before taking the action that depends on the output. | The interface communicates the specified limitation before that action. |
| AI-03 · Users can correct the result | Users or the system can apply, send, or save AI output or use it to make a decision | Run. Supply a deliberately incorrect result in a test environment. | Before application, the user can reject or correct the result. If the system applies it automatically, the documented path to correct or undo it works for the same example. |
| AI-04 · Users can see the external action before approval | AI prepares a payment, outbound submission, access grant, or irreversible deletion that requires separate approval | Run. Proceed to the approval step without confirming. | The interface shows the action and its target or recipient. No external effect occurs before approval. |

AI-04 applies to modes that require approval. [Microsoft Research Guidelines for Human–AI Interaction](https://www.microsoft.com/en-us/research/blog/guidelines-for-human-ai-interaction-design/)

## Testing with the target audience

An agent can run a flow and verify interface state. Use observational data to determine how easily people find an action, complete a task, or perceive a visual property. Before testing, record roles, tasks without hints, scoring rules, sample size, and thresholds for each flow or segment. Applicability depends on the audit goal. When the audit aims to assess the audience's experience, use `not_checked` if research data is unavailable.

| ID · Check | Applies when | Test | Yes when |
| --- | --- | --- | --- |
| RES-01 · Users reach the goal | A flow has a target unassisted completion rate | Research. Calculate the share of valid attempts that achieve the specified outcome without help. | The share meets or exceeds the agreed threshold in each required segment. |
| RES-02 · Users complete the flow within the time budget | A flow has a time budget and a rule for counting incomplete attempts | Research. Measure the time from task assignment to completion or the established time limit. | The agreed time metric stays within the threshold in each required segment. |
| RES-03 · Users find the first action | A flow has a set of acceptable first actions and a target share | Research. Assign the task at the starting screen and record each first meaningful action using the rule defined in advance. | The share of acceptable first actions meets or exceeds the threshold in each required segment. |
| RES-04 · Audience ratings of a visual property meet the target | One visual property has been defined, along with a question, scale, and threshold, such as for perceived visual clarity | Research. Show the specified screen in the same context and ask members of the audience the same question. | The response statistic chosen in advance reaches the threshold in each required segment. |
| RES-05 · Perceived ease meets the target | A flow has an ease question, scale, and threshold | Research. Collect a rating on the agreed scale after each valid attempt. | The rating statistic chosen in advance reaches the threshold in each required segment. |
| RES-06 · First-time readers identify the view's purpose | An audit defines the intended purpose, accepted answers, starting view, exposure, and target share for first-time readers | Research. Show the starting view with no walkthrough for the agreed exposure, then ask what it is for. Score each unprompted response against the accepted answers. | The correct-answer share meets the agreed threshold in each required segment. |
| RES-07 · First-time readers find and interpret a fact | A view has a defined information question, accepted answers including required qualifications, task limit, and target share | Research. Give first-time readers the neutral question from the agreed entry state. Record their answers and elapsed time without naming the location or control. | The share that gives a correct and complete answer within the task limit without help meets the agreed threshold in each required segment. |

Audience research references. [GOV.UK Moderated Usability Testing](https://www.gov.uk/service-manual/user-research/using-moderated-usability-testing), [GOV.UK Usability Benchmarking](https://www.gov.uk/service-manual/measuring-success/usability-benchmarking-a-website-or-whole-service), [ISO 9241-11](https://www.iso.org/standard/63500.html)
