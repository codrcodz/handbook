# Infrastructure-as-code Guidance


## Guiding Principles

Infrastructure should be provisioned, configured, and have its lifecycle managed from code defined as Infrastructure-as-code (IaC), whenever possible. This provides the following advantages:

  - Repeatability
  - Builtin Documentation
  - Improved change control process
  - Configuration auditing
  - Automation
  - Rapid recovery after system failures

Any deviation from the IaC principle should be carefully considered by the team to ensure that manual manipulation of the infrastructure warrants the technical debt it creates before this option is exercised.

In order to acheive this, IaC contributors should:

  - Use source code management (SCM) & Peer Reviews
  - Favor idemotency
  - Thoughtfully select tooling
  - Ensure consistency through testing
  - Adhere to best practices
  - Strive for cross platform support
  - Design code for maintainability

## Use Source Code Management (SCM) & Peer Reviews

IaC should be stored in a source code management system so that it may be subject to both version control and peer review. IaC reviewers should only provide peer review for code that they can read, understand, and provide feedback for. IaC contributors should not request peer reviews from peers that are unable to read, understand, or provide feedback for their code. IaC contributors should follow workflow, pull/merge request standards, and other administrative requirements defined for the management of IaC repositories as follows.

  - When applicable, peer reviews should be enforced via controls in the source code management system on repositories that will be used to deploy production infrastructure.
  - Code standards should follow a set of relatively simple, well-defined, and commonly used community best-practices.
  - Code standards should be enforced in an automated fashion with automated testing.
  - Overly complex workflows should be avoided and complexity should only be introduced if the team agrees that the benefits warrant the administrative overhead and that each piece of added complexity adds sufficient value.


## Favor Idempotency

IaC will be written in a way that is idempotent; regardless of the programming language or tooling, IaC contributors should ensure that their code only makes changes to infrastructure when required and has the appropriate conditionals in place to detect when/if a change to the infrastructure is required, before making it. When using tooling that natively supports idempotent logic but provides the capability to write non-idempotent logic, the idempotent features should be favored. Any code that is not idempotent should not run implicitly and should have to be toggled-on via a conditional to ensure it is never accidentally run. Clear instructions should be provided by contributor to indicate when and when not to run the non-idempotent code.


## Thoughtfully Select Tooling

Infrastructure-as-code tooling and language selection should be made based on the following attributes. The tooling should: 
  - Be flexible enough to accommodate as much of the infrastructure of various hardware and operating system types found in the network it is being used in as reasonable
  - Not require a persistent agent/daemon to run on the system in order to function
support language(s) and or domain-specific language(s) that are simple to use and have a low learning curve for a person with basic scripting knowledge
  - Natively support cross-platform languages leveraged for extensions and customizations
  - Has a low barrier of entry for individuals that may be unfamiliar with IaC tooling, but have a good foundational knowledge of scripting

Each of these desired attributes avoids a reliance on tooling that is inflexible and unable to change with shifting requirements or personnel changes.


## Ensure Consistency Through Testing

IaC should be testable and repeatable. IaC contributors should ensure that their code includes instructions and/or automation for conducting smoke tests, at a minimum. If possible, code should be linted, unit and integration tested, and functionally evaluated. If possible, this testing should be performed as consistently and in a fashion that is as automated as possible, reducing the chance for human error.


## Adhere to Best Practices

IaC tooling users should use best practices defined by either the tool vendor and/or subject matter experts within the user community. Once a tool has been included as part of the team's IaC tool suite, the team should define and document standards on how that tool will be used. While following the upstream best practices should be the default behavior, any deviation from them should only be considered when there is a compelling reason to do so and when it is deemed that it is worth the deviation; following upstream best practices will enable rapid on-boarding of new personnel by allowing those with existing tool knowledge to begin work immediately as well as faster onboarding for those that lack any knowledge of the tooling, but have access to upstream documentation, or derivative documentation, training materials, and/or demonstrations that leverage the upstream documentation as their reference material.


## Strive for Cross Platform Support

While every effort should be made to ensure our infrastructure is homogenous, IaC contributors should be prepared for the possibility of a heterogenous production environment; therefore, IaC logic should be built-in to detect the platform it is being run against. It should either run the appropriate code for the target platform or fail quickly to avoid unintended changes on any unsupported platforms.

In the event that the inclusion of the unsupported platforms warrants the capability it gains the team, despite the additional overhead, the team should be willing to adopt the new platform.

Examples of types of platforms that may warrant inclusion include: 

  - Solutions shipped as an “appliance” by the vendor that are mature and useful enough to warrant using the appliance without a large config management overhead
  - Applications that are very well tested on an operating system that is not currently supportable by our IaC tool suite
  - Solutions where the platform is supported, but is considered ephemeral and not ideal for direct management
  - Solutions managed by another team


## Design Code for Maintainability

IaC contributors should write code and configurations in a way that follows common patterns that are proven to make code more readable and maintainable. To accomplish these goals, IaC contributors should follow these principles when possible:

  - Use sensible default values in your code wherever a reasonable default value can be set
  - Separate data from code; data should be fed into the code, not nested within; data unique to the environment like, hostnames, IP addresses, other environment specific values should be defined in variables outside the executing code
  - Use the Don’t-Repeat-Yourself (DRY) principle; use modular design by grouping your code into executable functions/subroutines that can be called multiple times within the code base, but are written in a single location
  - Document code with README docs and/or inline comments whenever the code is doing anything other than the most generic or routine of actions
  - When given the option to provide an arbitrary name to something in your codebase (i.e. variables, functions, etc.), use a meaningful name, especially if the name will be referenced multiple times and/or it is not defined close to where it is used in the codebase. 
