# azure-pipelines-templates
Collection of CodeQL Examples

Azure pipelines allow for the creation of templates that can be referenced inside other pipelines. This encourages the creation and usage of re-usable code. Azure recommends this method for ensuring a unified approach to pipelines.

Azure pipelines support both #includes and #extends pipelines. While #includes is the equivalent of pasting the contents of the template into the outer file, #extends, functioning like inheritance, is the more secure approach as it provides scaffolding preventing the execution of malicious code.

[Security Through Templates](https://learn.microsoft.com/en-us/azure/devops/pipelines/security/templates?view=azure-devops)

## Codeql example

We recommend this approach for codeql. The steps for codeql execution never change. Download the binary, create the database, analyze, and upload. The only thing that changes is the parameters passed into these arguments. As an example, two of these parameters might be the language and the query packs to execute. These can be denoted like this:
```yaml
parameters:
  - name: runExtendedSecurity
    default: true
    type: boolean
  - name: language
    type: string
    default: javascript
    values:
    - javascript
    - python 
```
We use these parameters accordingly when defining our steps.
```yaml
  - script: |
      codeql database analyze ${{parameters.language}}-db  \
      ${{parameters.language}}-code-scanning.qls \
      --format=sarif-latest \
      --output=$(sarif-file-name).sarif
    displayName: 'Populate the CodeQL runner databases and analyze them'
```
One single template can now be used for all tech stacks. It is recommended that there remain a standard templates repository for templates like these to be used.
