---
layout: subpage
---

# Scenario metadata

Scenario metadata specifies the details of the scenario, including instructional text for the user, how the scenario should be displayed and what containers and command should be run. Whilst this adds some complexity over using raw OCI images, it means that a single container image can often be reused to implement multiple scenarios.

Metadata is provided via a Yaml document, `scenario.yaml`. The basic sections are:
* `group` - The group of the scenario, used to group scenarios in a series **Default:** None
* `name` - The name of the scenario
* `category` - The category of the scenario
* `description` - A markdown based description of the scenario used to inform the user of the scenario goals
* [`executor` - Detailed on the 'Executors' page](/scenario/executors.html)
* `conf`
  * [`commands` - Detailed on the 'Commands' page](/scenario/commands.html)
  * [`expose` - Detailed on the 'Expose services' page](/scenario/expose.html)
  * [`flags` - Detailed on the 'Flags' page](/scenario/flags.html)
* [`display` - Detailed on the 'Display' page](/scenario/display.html)

See the [Simple](/examples/simple.html), [Complex](/examples/complex.html) and [Virtualised](/examples/virtualised.html) scenarios for example implementations.
