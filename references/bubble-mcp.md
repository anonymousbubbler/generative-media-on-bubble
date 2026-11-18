# Working through Bubble MCP

Connect to `https://mcp.bubble.io/mcp` using your assistant's supported integration. The [official setup guide](https://manual.bubble.io/help-guides/ai/bubble-mcp) covers ChatGPT, Codex, Claude and other clients. Bubble documents native editor changes, plugin installation and development previews; deployment remains a manual editor step. Verify current support and authorization in the connected session.

## Discover the current surface

Read the server instructions and relevant `read_guide` topics before edits. Tool names below identify the currently advertised capability families; tool arguments must come from the actual connected schema, not this document.

1. Establish identity and target using `whoami`, `list_apps`, `get_versions` and the user's editor URL. Do not silently create a different app when the supplied one is inaccessible.
2. Read `get_app_outline` and selected `get_node` results. Inspect the relevant `list_types`, plugin detail and plan context. This is structural discovery; do not read unrelated records.
3. Use `get_element_type_fields`, `list_action_types`, `list_event_types` and `get_expression_vocabulary` for exact native field/action/expression formats. Validate typed expressions before writing fragile conditions.
4. Check BYOK plugin availability and installation entitlement with the plugin discovery/install tools. A plugin-editor URL is not an installation URL. Verify its actions after installation.
5. Preserve a savepoint or scoped before-state. Use supported batch changes for related edits when appropriate; otherwise use the specific creation/update tools. Read back the results and reconcile uncertain writes before retrying.
6. Use fresh issue checks and `get_preview_link`. Open the preview and exercise the feature. Adding test-case descriptions does not run tests.

Treat data access grants, live deployment, plugin purchase and credentials as separate operations requiring the user's actual authorization. Do not weaken privacy merely to allow an agent's record tools to work.

## What to record

Save `.fal-bubble/installation.json` in the customer's project, without credentials or customer records. Include:

- App and development version, inspection date and installed plugin version.
- Page/reusable, element, workflow, data type and field IDs actually returned by Bubble.
- Selected endpoint and schema source/date; input and output mappings.
- Ownership/tenant rule, file visibility and transfer method.
- Recovery point, relevant change receipts and preview URL.
- Checks performed, whether any inference was billable, and unresolved dependencies.

On the next request, verify these references against the current app and reuse them. A missing or renamed node is a reconciliation problem, not a reason to install duplicate components.

## Missing capabilities

Name the specific missing step and continue independent work. For example, prepare the workflow while the user enters a private key, or share the marketplace link if the app is missing the connector. Never claim that a connected service guarantees every operation is permitted. This starter uses the official MCP surface and intentionally includes no undocumented editor injection runtime.
