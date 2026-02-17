# d365-autopilot Constitution
<!--Arineo Dynamics 365 Finance and Operation Development Guideline-->
The primary goal of this guideline is to provide a set of best practices and coding standards to ensure that the resulting code is clean, efficient, reliable, maintainable, and meets the specific business requirements. This guideline MUST be followed.

## Core Principles

### I. [MUST] NAMING CONVENTION
Use self-explanatory ‘speaking’ names that represent the function or intended use of the object.
The code must comply with the naming conventions defined in the following table.
REPLACE the xxx with your projects actual prefix or give directions for the developers how to proceed if multiple prefixes are used.

| Application Object | Identifier |
|--------------------|------------|
|New table | XXXMyTable |
|New field in existing table | XXXMyField |
|New field in new table | MyField |
|New field in table extension | XXXMyField |
|New method in existing table | xxxMyMethod |
|New method in new table | myMethod |
|New method in extension class | xxxMyMethod |
|Extension for tables, forms e.g. SalesTable | SalesTable.ModelName (as set per Default) |
|Extension class for table e.g. SalesTable | SalesTable_XXX_Table_Extension |
|Extension class for class e.g. SalesTableType | SalesTableType_XXX_Class_Extension |
|Extension class for data entity e.g. CustomersV3 | CustomersV3_XXX_DataEntity_Extension |
|Extension class for form data source e.g. data source SalesTable of form SalesTable | SalesTable_XXX_FDS_SalesTable_Extension |
|Extension class for form data source field e.g. field SalesLine.ItemId of form SalesTable | SalesTable_XXX_FDSF_SalesLine_ItemId_Extension |
|Extension class for form control e.g. control GridHeader of form SalesTable | SalesTable_XXX_FCtrl_GridHeader_Extension |
|Context class for table method e.g. SalesTable.validateWrite | SalesTable_XXX_TableContext_ValidateWrite |
|Context class for class method e.g. SalesTableType.validateWrite | SalesTableType_XXX_ClassContext_ValidateWrite |
|Eventhandler classes| SalesTable_XXX_Table_EventHandler / SalesTable_XXX_Form_EventHandler / etc. |
|Interfaces | XXXICustVendTrans |
|Test classes | [Original objectname]Test -> i.e. XXXMyClassTest |
|Testable classes | [Original objectname]Testable -> i.e. XXXMyClassTestable |
|Test methods | [Method name or part of it][Scenario][Expected Result]Test -> i.e. newVendorPostedTypeIsSetToProspectTest |
|Staging table | Add the suffix Staging to the staging table name e.g. TableNameStaging |
|Solution Name | [DevOpsID] -> i.e. 000109 |
|Project name in Solution | [DevOpsID]_ModelName -> i.e. 000109_SECApplicationSuiteExtension -> The Model ist optional and only needs to be set if you have multiple models in one solution |

### II. [MUST] Code Style and Best Practices
[MUST] General Coding Guidelines
- Keep your code efficient, clean and readable.
- The code must be free of compiler warnings and errors.
- The language for development is English (en-us). All names, comments and documentation headers must be written using this language.
- The DRY principle, which is stated as "Every piece of knowledge must have a single, unambiguous, authoritative representation within a system", should be observed.
- The single-responsibility principle should apply to all objects created during development.
    - "A functional unit on a given level of abstraction should only be responsible for a single aspect of a system’s requirements. An aspect of requirements is a trait or property of requirements, which can change independently of other aspects.” – Ralf Westphal

[MUST] Microsoft Best Practices
Microsoft defines a wide range of best practices for Microsoft Dynamics 365 FO development. Most of those best practice rules can be automatically checked by the X++ compiler and result in errors or warnings. For all models used in development the automatic check of best practice must be activated. Checks of specific Best Practice rules must not be deactivated without alignment with the development lead. If any rules will be deactivated those exceptions will be distributed via version control to the development machines and should be listed in this document.

Beside these rules there are many that can’t be checked by the compiler, but nonetheless needs to be followed. The code must comply with the following rules:
- Best practice errors and warnings detected by the compiler
    - In justified cases suppressions may be used via a BPSuppressions file or using the SuppressBPWarning attribute.
- Best practices defined by Microsoft. Unfortunately, there is no dedicated list provided for D365FO, as the coding standards are spread throughout the Microsoft documentation. Nevertheless, the Microsoft coding standard always apply unless explicitly stated in this document.
    - By default, this document overrides the Microsoft rules if there are any conflicting rules.
- Additional rules defined in the following chapters

[MAY] Deactivated Best Practice rules
- Microsoft.Dynamics.AX.Framework.MaintainabilityRules -> SecurityPrivilegeRules -> BPErrorPrivilegeNotCoveredByDuty.
    - Reason for deactivation: During the initial development of new functionality and designs we will only create the security privileges. Duties and Roles will be defined later.

[MUST] Treatment of Microsoft and third-party code
Compiler errors and warnings in Microsoft or third-party code (such as ISV solutions) can't and shouldn't be fixed. Instead, these errors should be reported to Microsoft or the third party for removal. However, if Microsoft or third-party code is copied and reused for any reason, the changes should be free of compiler errors and warnings and the developer is responsible for the quality of the code.

[MUST] Coding standards
- Do not use creation comments (e.g. // Created on 2024-09-30 Task 123) or change comments (e.g. // Changed on 2024-09-30 Task 123 - Begin).
- Do not use column style.

[SHOULD] Appropriate Use of Comments in Code
Commenting within code should in general be minimal. However, if a piece of code is not clearly understandable a explaining comment should be placed.

Good - this following comment shows that the developer intentionally chose not to handle the else case, so the reviewer and future readers understand that this decision was deliberate
...
// Continue on validation error
if (this.validateWrite() == true)
{
    this.update();
}
...

Bad - this following comment is not necessary
...
int sum = a + b; // Sums a and b
...

[SHOULD] Handling of more than 2 parameters
Microsoft coding standards define that if there are more than two parameters, each parameter should be moved onto a new line and indented by 4 spaces. In general, this rule is in effect, but it might be omitted, if wrapping the parameters rather impairs the readability of the code than improving it. Rule of thumb: if the resulting code would be “higher” (in terms of lines) than it is “wide”, the rule can be omitted. An example is the use of built-in X++ functions which often take multiple integer parameters.

// Format according to MSFT coding standards
// This format is OK
info(substr(
  text,
  0,
  3);

info(num2str(
  number,
  10,
  4,
  0
  0);

// Format omitting MSFT coding standards
// This format is OK too!
info(substr(text,  0, 3);

info(num2str(number, 10, 4, 0, 0);

[SHOULD] Extensions and Chain of command vs. Event handlers
The use of Chain of Command should be preferred before the use of event handlers. Static event handlers should not be created in extension classes but in a separate class. For non static event handlers the same is recommended.

[MUST] Ternary operators
The following additions apply to the use of ternary operators:
- They do not span multiple lines.
- They are not nested.
- Keep them as short and readable as possible.

MAY (Example) Ternary operators
    // This is ok
    dateStatus = inventTrans.DateInvent ? inventTrans.DateInvent : inventTrans.DateExpected;

MUST NOT (Example) Ternary operators
// MUST NOT
    dateStatus = inventTrans.DateInvent ? inventTrans.DateInvent
       : inventTrans.DateExpected;

    // MUST NOT
    dateStatus = inventTrans.DateInvent ? inventTrans.DateInvent
       : inventTrans.DateExpected ? inventTrans.DateExpected : inventTrans.DatePhysical;

Boolean expressions
Boolean expressions should be written out in full.
MAY (Example)
    // This format is OK
    if (inventTrans.RecId)
    {
     // do something
    }

    if (!inventTrans.InvoiceId)
    {
       // do something
    }

SHOULD (Example)
    // This format is better!
    if (inventTrans.RecId != 0)
    {
       // do something
    }

    if (inventTrans.InvoiceId == '')
    {
       // do something
    }

If the condition of an if, else if or while block comprises a complex boolean expression then the following rules must be met:
- Each sub expression is found on a separate line
- The boolean operators are found at the beginning of a line
- Simple sub expressions come first i.e. ranges on table fields before method calls
- The second and all following sub expressions are indented by one level regarding the if, else if or while expression
- Further indentation is needed if the boolean expression requires the use of brackets. In doing so the maximum indentation is one level. If the use of brackets requires a higher indentation, then the complexity of the boolean expression must be decreased by refactoring code into methods.

MUST
    if (salesLine.ItemId == '0815'
        && salesLine.SalesQty != 0
        && (salesLine.SalesStatus == SalesStatus::Backorder
            ||salesLine.SalesStatus == SalesStatus::None))
    {
        // do something
    }

MUST NOT
    if (salesLine.ItemId == '0815'
        && ((salesLine.SalesStatus == SalesStatus::Backorder
                && salesLine.RemainSalesPhysical == 0)
            || (salesLine.SalesStatus == SalesStatus::None)
                && salesLine.SalesQty != 0))
    {
        // do something
    }

MUST Refactor into:
    if (salesLine.ItemId == '0815'
        && this.checkSalesLineStatus(salesLine))
    {
        // do something
    }


[MUST] Database queries
Queries to the database (e.g. select statements) must be created so that the conditions use indexes effectively. If no matching index exists, check if the query can be made more efficient by using joins. If this isn’t possible either, then consider creating a new index or maybe a link table with respective indexes. The following rules apply to the formatting of select und while select statements:
- The key words from, group by, order by, index, index hint, where, join, exist join, notexists join and outer join always start a new line which is indented by one level in relation to the superordinate select, while select, join, exist join, notexists join or outer join key word.
- If the where block comprises multiple conditions, then each condition has to be on a separate line and the boolean operators are found at the beginning of a line.
- The second and all following conditions are indented by one level in relation to the where key word. Further indentation is needed if the conditions require the use of brackets.
    while select sum(Qty)
        from inventTrans
        order by Statuslssue
        where inventTrans.Itemld == '1234'
            && inventTrans.StatusReceipt == StatusReceipt::None
            && (inventTrans.StatusIssue == Statuslssue::Deducted
                || inventTrans.StatusIssue == Statuslssue::SoId))
        join inventDim
            group by InventSizeId
            where inventDim.InventDimId == inventTrans.InventDimId
            exists join inventLocation
                index hint InventLocationIdx
                where inventLocation.InventLocationId == inventDim.InventLocationId
                    && inventLocation.InventLocationType == InventLocationType::Standard
    {
        // do something
    }

[MUST] Nesting
The nesting of code by using if-else, switch, for, while, do-while, try-catch, changeCompany or while select statements must not exceed three logical levels. If needed, try to refactor part of the code into separate method(s). The nesting of switch statements isn’t allowed.

[MUST] Methods
In accordance with Microsoft best practices all methods should be as short as possible and responsible for one specific task (Single-responsibility principle), which is reflected in the methods name.
You should keep your methods short. If they get too long, you should consider splitting them into several methods. The line of X++ code should also not be too wide. If you have to scroll to see the end of the line, it is too long!
Declare your variables at the innermost scope possible.
If your method is an override method or a 'chain of command' wrapper method, it should only contain a call to another method where your code should be placed. This should be done to enable unit-testing and provide extensibility.

[SHOULD] Local functions
The best practice is to add private methods to the class rather than to add local functions inside the method. If a local function is used anyway then the following recommendations apply:
- The local function should be as short as possible.
- The local function shouldn’t modify variables of the superordinate method.
- The local function shouldn’t be longer than the superordinate method.

[MUST] XML documentation header
Every new method and new class needs an XML documentation header. When using overwrite methods e.g. init(), active(), etc. create a XML documentation if best practice check expects one. If not, you either create an XML documentation or you can use the comment:
- //Inherited from parent method

[SHOULD] ToDo comments
Todo comments should only be used during development and must be removed before check-in. Exceptions can be made if the ToDo comment is made in regards of a requested fix or extensibility request from Microsoft or an ISV, where we only need specific code until we get the solution from them.

[SHOULD NOT] Macros
Macros should no longer be used. Macros are supported for backwards compatibility only. Instead of macros, use language constructs like these:
- Constants
- SysDa for queries

Classes
The use of the SysOperation Framework should be preferred before the use of the RunBase framework.

Forms
All newly created forms must specify a pattern. Also, all new form controls that allow the specification of a pattern (e.g. tab pages or groups) should do so. The custom pattern should only be used on rare occasions.

Methods in forms should only control the user interface. Business logic must be placed in class or table methods.

Tables
For tables you must follow this rules:
- Each new field must use an extended data type or base enum. If no fitting one is available, create a new one.
- Each new (non-temporary, non-staging) table with a unique index should have corresponding find and exist methods.
    - If needed, create additional, specialized find methods instead of using inline SQL code. (e.g. findByRecId)
- For the initialization of multiple fields from another table, create an initFrom<buffer> method.

Table indexes
Table indexes are crucial for the system performance. While there are no general rules about when to create indexes and which fields to include in what order, please consider the following.
- Assign a unique index to each table if possible.
- Strongly consider designating one of the indexes as the cluster index.
- If no existing index can be used in a heavily used SQL statement create an index. If it’s only rarely used, DON’T! They decrease performance on inserts, updates, and deletes.
- Consider the included columns feature! (https://community.dynamics.com/365/financeandoperations/b/goshoom/posts/included-columns-in-dynamics-ax-2012)
- Updating indexed fields is time consuming and the order of the fields matter. It is more time-consuming to change fields at the beginning of an index than at the end of an index. If fields in an index are updated frequently, consider placing these at the end of the index.
- The order of index fields matters also in regards of how selective the fields are. Like in a telephone book, entries are ordered first by last name, then by first name. But keep in mind that how the table is queried and which fields are most used to query it may contradict this point.

[MUST] Security
The following security artifacts must be created, depending on the objects created during development.
- For each display menu item:
    - A privilege with read access, and an appropriate name and suffix “View”
    - A privilege with delete or otherwise suitable access, and an appropriate name and suffix “Maintain”, when the respective form allows the maintenance of the displayed data
- For each action menu item:
    - A privilege with delete access and an appropriate name
- For each output menu item:
    - A privilege with read access and an appropriate name
- For each data entity:
    - If the data entity is intended for data management:
        - A privilege with read access, integration mode DataManagement and suffix “Export”
        - A privilege with create access, integration mode DataManagement and suffix “Import”o
    - If the data entity is intended for general integration via OData or Microsoft Office:
        - A privilege with read access, integration mode DataServices and suffix “View”
        - A privilege with delete access, integration mode DataServices and suffix “Maintain”o
    - If the data entity is intended for both:
        - A privilege with read access, integration mode All and suffix “View”
        - A privilege with delete access, integration mode All and suffix “Maintain”

[MUST] Data model changes
Renaming a field or changing its datatype results in loss of the data stored in those fields. Therefore once a field is deployed to PROD and is in use, it shouldn’t be renamed and the datatype shouldn’t be changed. If such a change is needed, please ensure that data loss is prevented. Depending on the type of the change and the related quantity of data use a matching approach, such as:
- Use a new field
    - Create a new field with the correct name and data type.
    - Mark the old field as Obsolete
    - Exchange the old field with the new one in the UI and the business logic.
    - Provide a way to transfer from the old field to the new one, (e.g. a data update job as a runnable class)
    - Plan and delete the old field in a follow-up release. (A related devOps work item should be created)
- Backup and restore the data
    - Ensure that a data entity exists to export the data from the changed tables and fields.
    - Export the data directly before deployment. (Have a task for this in the release plan!)
    - Ensure that the data entity will be changed to make use of the new field with the same release, that contains the data model change.
    - Re-import the data into the new field directly after the deployment. (Have a task for this in the release plan!) If the field is not yet deployed to PROD, but to test environments as e.g. UAT and is used there, please ensure to either take one of the approaches described above or ensure that the loss of the data is acceptable for the team. If the field is only used on development- and the sprint test environments, please inform the involved persons and clarify if the loss of data is acceptable.

If any changes to a unique index is needed, please ensure that no database synchronization issues will happen or take the necessary mitigation steps. Please inform affected people of such changes so that the mitigation steps will be done during deployment and that other developers are informed about what to do to keep their development environments functioning.

[MUST] Use of Visual Studio solutions/projects
For each design or bug a Visual Studio solution must be created. The solution name should be the Id of the DevOps work item that will be associated with the check-in of the code. For each model where an AOT object is created or customized, a separate project must be added to the solution. As stated in the naming conventions the Id of the work item should be used as solution name. The project name for our default development model should also be the work item id. If further projects are necessary, please use [DevOpsID]_[Modelname] as project name.

[MUST] Check-in code to Version control
It must be ensured that each check-in in the version control system only contains changes for exactly one PBI or bug. Checking in changes that include several independent developments is not permitted. The work item associated with the check-in should be REPLACE: (ToDo: Please specify if the code should be associated with the PBI/Bug or the Development task or both ) the PBI that was implemented or the bug that was fixed. Only items added or changed for that specific work item should be included in this check-in. The check-in comment should include the ID and preferably the title of the implemented work item, or in the case of subsequent check-ins for the same work item, a brief description of the changes made or the reason for those changes.

[SHOULD] Unit Tests
All unit tests must be in a separate model. Per model used in the project we will have one test model. • E.g: Model: MYMODEL Testmodel: MYMODELTest Related Microsoft Documentation and further details:
- SysTestFramework
- Acceptance test library

## Governance
<!-- Example: Constitution supersedes all other practices; Amendments require documentation, approval, migration plan -->

[GOVERNANCE_RULES]
<!-- Example: All PRs/reviews must verify compliance; Complexity must be justified; Use [GUIDANCE_FILE] for runtime development guidance -->

**Version**: [CONSTITUTION_VERSION] | **Ratified**: [RATIFICATION_DATE] | **Last Amended**: [LAST_AMENDED_DATE]
<!-- Example: Version: 2.1.1 | Ratified: 2025-06-13 | Last Amended: 2025-07-16 -->
