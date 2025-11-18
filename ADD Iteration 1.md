# Step 1: Review Inputs

<table>
  <tr>
    <th>Category</th>
    <th>Details</th>
  </tr>

  <tr>
    <td><b>Design Purpose</b></td>
    <td>
      This is a greenfield system in a mature domain. The purpose is to produce a sufficiently detailed design to support the construction of the system.
    </td>
  </tr>

  <tr>
    <td><b>Primary Functional Requirements</b></td>
    <td>
      From the use cases created during phase 1, the primary use cases are listed below:<br><br>
      <b>UC-1:</b> Core functionality that the platform can't work without<br>
      <b>UC-2:</b> Key function that is required in order for the platform to be considered operational<br>
      <b>UC-6:</b> Main purpose of the platform is to implement this function<br><br>
      These three use cases need to be implemented before anything else in order for the platform to work.
    </td>
  </tr>

  <tr>
    <td><b>Quality Attribute Scenarios</b></td>
    <td>
The quality attribute scenarios from phase 1 are listed below, each with its own ranking for importance and difficulty to implement.<br><br>

| Scenario ID | Importance to Customer | Difficulty of Implementation |
|-------------|------------------------|------------------------------|
| QA-1        | High                   | High                         |
| QA-2        | High                   | High                         |
| QA-3        | Medium                 | Medium                       |
| QA-4        | Medium                 | High                         |
| QA-5        | High                   | High                         |

From the table above, the selected drivers are <b>QA-1</b>, <b>QA-2</b>, <b>QA-3</b>, and <b>QA-5</b> because they directly support the primary functional requirements for this iteration.
    </td>
  </tr>

  <tr>
    <td><b>Constraints</b></td>
    <td>
      CRN-1, CRN-3, and CRN-4 are included since they are required for the architectural design. CRN-2 can be deferred to a later iteration.
    </td>
  </tr>

  <tr>
    <td><b>Concerns</b></td>
    <td>
      All of the concerns created from phase 1 are included since they all are required for the architectural design.
    </td>
  </tr>

</table>

# Step 2: Establish Iteration Goal by Selecting Drivers

Since this is the first iteration, **we want to establish the overall system structure**. In this case we must be mindful of all the inputs, but pay special attention to the following:

- QA-1: Security/privacy
- QA-2: Perfomance
- CON-1: Mandatory connectivity
- CON-3: Implement SSO
- CON-4: In sync with university systems data
- CRN-2: Monitoring system and logging failures
- CRN-3: Cloud nativity

![Context Diagram for step 2 of ADD process iteration 1](/iteration-artifacts/iteration1_context.drawio.png)

# Step 3: Choose One or More Elements of the System to Decompose

Because we are developing a greenfield system, we will be refining the entire AI-Powered Digital Assistant Platform (AIDAP). Therefore, **the platform will be refined through decomposing the entire system**.

# Step 4: Choose One or More Design Concepts That Satisfy the Selected Drivers



# Step 5: Instantiate Architectural Elements, Allocate Responsibilities and Define Interfaces



# Step 6: Sketch Views and Record Design Decisions



# Step 7: Perform Analysis of Current Design and Review Iteration Goal and Achievement of Design Purpose

