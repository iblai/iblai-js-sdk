# Documentation

# IBL AI JavaScript SDK

The ibl.ai JavaScript SDK is a powerful toolkit designed to seamlessly integrate with the IBL (Innovation Business Learning) ecosystem. This SDK enables developers to build sophisticated, AI-powered e-learning platforms by providing easy access to IBL's comprehensive suite of APIs.

Key features include:

- Course management and enrollment
- Learning resource handling
- User authentication and profile management
- Skill assessment and tracking
- Learning pathway creation and management
- Integration with IBL platform courses and micro-frontends
- Payment processing via Stripe

With this SDK, developers can rapidly create feature-rich, adaptive learning experiences that leverage the full potential of AI in education.

For detailed API usage, refer to the API section below.

## INTEGRATION

### Step 1: Create an HTML File

Start by creating a basic HTML file for your web application:

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>IBL AI Web App</title>
    <script
      type="text/javascript"
      src="https://ibl.ai/sdk/js/main.js?versionId=_7n6hek72SK.pAhbtFDqDVv.dSmb8vmx"
    ></script>
  </head>
  <body>
    <h1>Welcome to IBL AI Web Application</h1>
    <div id="app"></div>
  </body>
</html>
```

### Step 2: Include the IBL AI SDK

In your main.js file, include the IBL AI SDK and create an instance of the API object. The API object allows you to interact with various endpoints for learning management, user authentication, and resource management.

```javascript
const api = new window.IblJsClient.default({
  IBL_DM_URL: "<DM_URL>", // The URL for the Data Manager system
});
```

Explanation of Configurations:

- `IBL_DM_URL`: The base URL of the Data Manager system for retrieving learning resources.

### Step 3: Authentication for Private Endpoints

To access certain private API endpoints, your users must be authenticated. Follow these steps to authenticate your users:

1. _Login to the IBL platform:_ Use the SDK's authentication method to log in to the IBL platform. Replace username and password with the actual user credentials.

```javascript
api.iblwebauth
  .login(formData<username_or_email, password>, successCallback, errorCallback)
```

2. _Fetch User Tenants and Retrieve Tokens:_ After login, get the user's tenants to identify which organization or platform the user belongs to.

```javascript
api.ibledxtenants.getUserTenants(
    (tenants) => {
        // logic to select tenant to log user into from list of tenants
        api.iblutils.saveUserTenantsDataToLocalStorage(
        tenants,
        <selected_tenant_key>
        );
        if (tenants.length) {
            const formData = new FormData();
            formData.append('platform_key', localStorage.getItem('tenant'));
            api.ibldmauth.getToken(
                formData,
                ({ data }) => {
                    api.iblwebauth.initializeLocalStorageWithAuthData(
                        data.axd_token,
                        data.dm_token,
                        data.user,
                        tenants,
                        localStorage.getItem('tenant')
                    );
                }
                // Now you have local storage populated with authentication data for the user
            )
        }
    })
```

#### Step 4: Fetch Data and Build Your UI

Now that the authentication is complete, you can fetch data using the SDK's API methods and build the user interface. Here’s an example of fetching courses and displaying them:

```javascript
api.ibldmcourses
  .getCourses()
  .then((courses) => {
    const coursesList = document.createElement("ul");
    courses.forEach((course) => {
      const listItem = document.createElement("li");
      listItem.textContent = course.name;
      coursesList.appendChild(listItem);
    });
    document.body.appendChild(coursesList);
  })
  .catch((error) => console.error("Error fetching courses:", error));
```

By following these steps, you can integrate the IBL AI SDK into your web application, authenticate users, and interact with learning resources and user data. The SDK provides a wide range of functionality, allowing you to build robust, AI-powered e-learning platforms.

## API

| API                 | Function                                                                                              | Description                                                                     |
| ------------------- | ----------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------- |
| iblaxdaiapi         | aiMentorOrgsUsersRecommendCoursesRetrieve(tenant, username, options callbackFunc)                     | Retrieves a list of recommended courses for a user                              |
| iblaxdaimentorapi   | aiMentorOrgsUsersList(tenant, username, callbackFunc)                                                 | Retrieves a list of users in a tenant                                           |
|                     | aiMentorOrgsUsersRecommendCoursesRetrieve(tenant, username, options, callbackFunc)                    | Retrieves a list of recommended courses for a user                              |
| iblaxdcredapi       | credentialsPublicAssertionsRetrieve(credentialId, callbackFunc)                                       | Retrieves details of a credential                                               |
|                     | credentialsOrgsUsersAssertionsOverTimeRetrieve(tenant, username, callbackFunc)                        | Retrieves assertions over time for a user in a tenant                           |
|                     | credentialsOrgsUsersCourseCredentialsList(tenant, username, callbackFunc)                             | Retrieves a list of course credentials for a user in a tenant                   |
|                     | credentialsOrgsUsersAssertionsRetrieve(tenant, callbackFunc)                                          | Retrieves credentials for a user in a tenant                                    |
|                     | credentialsOrgsUsersRetrieve(tenant, username, , callbackFunc)                                        | Retrieves credentials for a user in a tenant                                    |
| iblaxdperlearner    | perlearnerOrgsUsersInfoRetrieve(tenant, username, callbackFunc)                                       | Retrieves user information for a specific tenant and username                   |
|                     | perlearnerOrgsUsersOverviewTimeOverTimeRetrieve(tenant, username, callbackFunc)                       | Retrieves user overview time data over time for a specific tenant and username  |
|                     | perlearnerOrgsUsersActivityRetrieve(tenant, username, callbackFunc)                                   | Retrieves user activity data for a specific tenant and username                 |
| iblaxdoverviewapi   | overviewOrgsAverageGradeRetrieve(tenant, data, callbackFunc)                                          | Retrieves average grade for a specific tenant                                   |
|                     | overviewOrgsMostActiveCoursesRetrieve(tenant, data, callbackFunc)                                     | Retrieves most active courses for a specific tenant                             |
|                     | overviewOrgsRegisteredUsersRetrieve(tenant ,startDate, callbackFunc)                                  | Retrieves registered users for a specific tenant and start date                 |
|                     | overviewOrgsActiveUsersRetrieve(tenant, startDate, callbackFunc)                                      | Retrieves active users for a specific tenant and start date                     |
|                     | overviewOrgsCoursesCompletionsRetrieve(tenant, startDate, callbackFunc)                               | Retrieves course completions for a specific tenant and start date               |
| iblaxdperformance   | performanceOrgsGradingPerCourseRetrieve(tenant, data, callbackFunc)                                   | Retrieves grading performance per course for a specific tenant                  |
|                     | performanceOrgsGradingAverageRetrieve(tenant, data, callbackFunc)                                     | Retrieves average grading performance for a specific tenant                     |
|                     | performanceOrgsCoursesGradingAverageRetrieve(courseId ,tenant, callbackFunc)                          | Retrieves average grading performance for a specific course and tenant          |
|                     | performanceOrgsCoursesGradingPerLearnerRetrieve(courseId, tenant, callbackFunc)                       | Retrieves grading performance per learner for a specific course and tenant      |
|                     | performanceOrgsCoursesGradingDetailRetrieve(courseId, tenant, callbackFunc)                           | Retrieves detailed grading performance for a specific course and tenant         |
| iblaxdreportapi     | reportsOrgsList(tenant, callbackFunc)                                                                 | Lists reports for a specific tenant                                             |
|                     | reportsOrgsNewCreate(tenant, reportData callbackFunc)                                                 | Creates a new report for a specific tenant                                      |
| iblaxdengagement    | engagementOrgsCourseCompletionOverTimeRetrieve(tenant, startdate, callbackFunc)                       | Retrieves course completion data over time for a specific tenant and start date |
|                     | engagementOrgsCourseCompletionPerCourseRetrieve(tenant, callbackFunc)                                 | Retrieves course completion data per course for a specific tenant               |
|                     | engagementOrgsTimePerCourseRetrieve(tenant, data, callbackFunc)                                       | Retrieves time spent per course for a specific tenant                           |
|                     | engagementOrgsTimeOverTimeRetrieve(tenant, startdate, callbackFunc)                                   | Retrieves time spent data over time for a specific tenant and start date        |
| iblaxdaudience      | audienceOrgsActiveUsersPerCourseRetrieve(tenant, startdate, callbackFunc)                             | Retrieves active users per course for a specific tenant and start date          |
|                     | audienceOrgsActiveUsersOverTimeRetrieve(tenant, startdate, callbackFunc)                              | Retrieves active users over time for a specific tenant and start date           |
|                     | audienceOrgsEnrollmentsOverTimeRetrieve(tenant, startdate, callbackFunc)                              | Retrieves enrollments over time for a specific tenant and start date            |
|                     | audienceOrgsRegisteredUsersPerCourseRetrieve(tenant, data, callbackFunc)                              | Retrieves registered users per course for a specific tenant and data            |
|                     | audienceOrgsRegisteredUsersOverTimeRetrieve(tenant, startdate, callbackFunc)                          | Retrieves registered users over time for a specific tenant and start date       |
| iblaxdskills        | skillsOrgsSkillsUsersPointPercentileRetrieve(tenant, username, callbackFunc)                          | Retrieves skill point percentile for a user in a specific tenant                |
|                     | skillsOrgsSkillsUsersRetrieve(tenant, username, callbackFunc)                                         | Retrieves skills for a user in a specific tenant                                |
| ibldmcourses        | createAttendance(data, successCallback, errorCallback)                                                | Creates attendance for a course                                                 |
|                     | getUserCourseCompletion(data, successCallback, errorCallback)                                         | Retrieves user course completion data                                           |
|                     | getCourseSkillPointInfo(data, successCallback, errorCallback)                                         | Retrieves skill point information for a course                                  |
|                     | getUserCourseCompletion(data, successCallback, errorCallback)                                         | Retrieves user course completion data                                           |
|                     | overviewOrgsCoursesCompletionsRetrieve(tenant, startDate, callbackFunc)                               | Retrieves course completions for a specific tenant and start date               |
| ibldmcredentials    | getOrgCredentials(data, successCallback)                                                              | Retrieves organization credentials                                              |
|                     | getUserCredentials(data, successCallback)                                                             | Retrieves user credentials                                                      |
|                     | getCredential(data, successCallback)                                                                  | Retrieves a specific credential                                                 |
|                     | getOrgIssuer(org, successCallback)                                                                    | Retrieves organization issuer                                                   |
|                     | createCredential(data, successCallback)                                                               | Creates a new credential                                                        |
|                     | getCourseCredentials(data, successCallback)                                                           | Retrieves course credentials                                                    |
|                     | issueCredential(issueId, data, successCallback)                                                       | Issues a credential                                                             |
|                     | getUserAssertion(id, successCallback)                                                                 | Retrieves user assertion                                                        |
|                     | getPublicAssertion(data, successCallback, errorCallback)                                              | Retrieves public assertion                                                      |
| ibledxmfe           | getMfeContext(successCallback, errorCallback)                                                         | Retrieves MFE context                                                           |
| ibldmresources      | getUserResourceCompletion(data, successCallback)                                                      | Gets user resource completion                                                   |
| ibldmplatform       | createCatalogInvitationsPlatform(data, successCallback, errorCallback)                                | Creates catalog invitations for a platform                                      |
|                     | createCatalogInvitationsCourse(data, successCallback, errorCallback)                                  | Creates catalog invitations for a course                                        |
|                     | createCatalogInvitationsProgram(data, successCallback, errorCallback)                                 | Creates catalog invitations for a program                                       |
|                     | createCoreUsersPlatforms(data, successCallback, errorCallback)                                        | Creates core users for platforms                                                |
|                     | getCorePlatformUsers(data, successCallback, errorCallback)                                            | Retrieves core platform users                                                   |
|                     | updateCorePlatformConfigSite(data, successCallback, errorCallback)                                    | Updates core platform configuration site                                        |
|                     | uploadResume(data, fromdata, successCallback, errorCallback)                                          | Uploads a resume                                                                |
|                     | getUserResume(data, successCallback, errorCallback)                                                   | Retrieves a user's resume                                                       |
|                     | fetchPlatformInvitations(data, successCallback, errorCallback)                                        | Fetches platform invitations                                                    |
|                     | fetchCourseInvitations(data, successCallback, errorCallback)                                          | Fetches course invitations                                                      |
|                     | fetchProgramInvitations(data, successCallback, errorCallback)                                         | Fetches program invitations                                                     |
|                     | createPlatformBulkInvitations(data, successCallback, errorCallback)                                   | Creates bulk invitations for a platform                                         |
|                     | createPlatformBlankInvitations(data, successCallback, errorCallback)                                  | Creates blank invitations for a platform                                        |
|                     | redeemPlatformInvitations(data, successCallback, errorCallback)                                       | Redeems platform invitations                                                    |
|                     | createCourseBulkInvitations(data, successCallback, errorCallback)                                     | Creates bulk invitations for a course                                           |
|                     | redeemCourseInvitations(data, successCallback, errorCallback)                                         | Redeems course invitations                                                      |
|                     | createProgramBulkInvitations(data, successCallback, errorCallback)                                    | Creates bulk invitations for a program                                          |
|                     | createProgramBlankInvitations(data, successCallback, errorCallback)                                   | Creates blank invitations for a program                                         |
|                     | redeemProgramInvitations(data, successCallback, errorCallback)                                        | Redeems program invitations                                                     |
|                     | fetchUserPlatformLink(data, successCallback, errorCallback)                                           | Fetches user platform link                                                      |
|                     | getPlatformConfigSitePublic(data, successCallback, errorCallback)                                     | Retrieves public platform configuration site                                    |
|                     | updateUserProgramStatus(data, successCallback, errorCallback)                                         | Updates user program status                                                     |
|                     | getRoles(data, successCallback, errorCallback)                                                        | Retrieves roles                                                                 |
|                     | createOrUpdateRole(data, successCallback, errorCallback)                                              | Creates or updates a role                                                       |
|                     | getDesiredRoles(data, successCallback, errorCallback)                                                 | Retrieves desired roles                                                         |
|                     | createOrUpdateDesiredRoles(data, successCallback, errorCallback)                                      | Creates or updates desired roles                                                |
|                     | getReportedRoles(data, successCallback, errorCallback)                                                | Retrieves reported roles                                                        |
|                     | createOrUpdateReportedRoles(data, successCallback, errorCallback)                                     | Creates or updates reported roles                                               |
| ibldmcred           | validateCredentials(userId, credentials)                                                              | Validates user credentials                                                      |
|                     | updateCredentials(userId, newCredentials)                                                             | Updates user credentials                                                        |
| ibldmpathway        | getPathway(data, successCallback, errorCallback)                                                      | Retrieves a specific learning pathway                                           |
|                     | createPathway(data, successCallback, errorCallback)                                                   | Creates a new learning pathway                                                  |
| ibldmprograms       | getPrograms()                                                                                         | Retrieves a list of available educational programs                              |
|                     | enrollInProgram(programId, userId)                                                                    | Enrolls a user in a specific program                                            |
|                     | updateProgramProgress(programId, userId, progressData)                                                | Updates a user's progress in a program                                          |
|                     | getUserPrograms(data, successCallback, errorCallback)                                                 | Retrieves user programs                                                         |
| ibldmskills         | getUserSkills(data, successCallback, errorCallback)                                                   | Retrieves user skills                                                           |
|                     | getUserSelfReportedSkills(data, successCallback, errorCallback)                                       | Retrieves user self-reported skills                                             |
|                     | getOrgSkills(data, successCallback, errorCallback)                                                    | Retrieves organization skills                                                   |
|                     | addSelfReportedSkill(data, successCallback, errorCallback)                                            | Adds a self-reported skill                                                      |
|                     | addDesiredSkill(data, successCallback, errorCallback)                                                 | Adds a desired skill                                                            |
|                     | getBlockSkillPointInfo(data, successCallback, errorCallback)                                          | Retrieves block skill point information                                         |
|                     | getUserSkillPointInfo(data, successCallback, errorCallback)                                           | Retrieves user skill point information                                          |
| ibldmauth           | getToken(data, successCallback, errorCallback)                                                        | Get the data manager and axd token for a user in a tenant                       |
| ibldmfeatures       | getApps(data, successCallback, errorCallback)                                                         | Retrieves a list of apps                                                        |
| ibldmusers          | getUserProfile(userId)                                                                                | Retrieves a user's profile                                                      |
|                     | updateUserProfile(userId, profileData)                                                                | Updates a user's profile                                                        |
|                     | deleteUser(userId)                                                                                    | Deletes a user account                                                          |
|                     | getUsersManageMetadata(data, successCallback, errorCallback)                                          | Retrieves users manage metadata                                                 |
| ibldmstripe         | checkoutSession(data, successCallback, errorCallback)                                                 | Creates a checkout session                                                      |
| ibledxcourses       | getCourseMeta(data, successCallback)                                                                  | Retrieves course metadata                                                       |
|                     | getCatalogCourses(data, successCallback, errorCallback)                                               | Retrieves catalog courses                                                       |
|                     | createCourse(data, successCallback)                                                                   | Creates a new course                                                            |
|                     | getMFEURL(data, successCallback)                                                                      | Retrieves MFE URL                                                               |
|                     | getAuthEdxIframeToken(data, successCallback)                                                          | Retrieves authenticated EdX iframe token                                        |
|                     | fetchUserCourses(data, successCallback)                                                               | Fetches user courses                                                            |
|                     | fetchEnrollStatus(data, successCallback, errorCallback)                                               | Fetches enrollment status                                                       |
|                     | enrollToCourse(data, successCallback, errorCallback)                                                  | Enrolls user to a course                                                        |
|                     | adminEnrollToCourse(data, successCallback, errorCallback)                                             | Admin enrolls user to a course                                                  |
|                     | unenrollFromCourse(data, successCallback, errorCallback)                                              | Unenrolls a user from a course                                                  |
|                     | get_user_course_grade_progress(data, successCallback, errorCallback)                                  | Retrieves a user's grade and progress for a course                              |
|                     | getCourseTabs(data, successCallback, errorCallback)                                                   | Retrieves the tabs available for a course                                       |
|                     | getCourseProgress(data, successCallback, errorCallback)                                               | Retrieves the progress of a user in a course                                    |
|                     | getCourseBlocks(data, successCallback, errorCallback)                                                 | Retrieves the block structure of a course                                       |
|                     | getStudentCourseContent(data, successCallback, errorCallback)                                         | Retrieves the course content for a student                                      |
| ibledxcourses       | getEdXCourses()                                                                                       | Retrieves a list of EdX courses                                                 |
|                     | syncEdXCourse(courseId)                                                                               | Synchronizes an EdX course                                                      |
|                     | mapEdXCourse(edXCourseId, internalCourseId)                                                           | Maps an EdX course to an internal course                                        |
| ibledxplatform      | getEdXPlatformStatus()                                                                                | Retrieves the status of the EdX platform                                        |
|                     | configureEdXIntegration(configData)                                                                   | Configures the EdX integration                                                  |
| ibledxpathway       | getEdXPathways()                                                                                      | Retrieves a list of EdX learning pathways                                       |
|                     | createEdXPathway(pathwayData)                                                                         | Creates a new EdX learning pathway                                              |
| ibledxpathways      | getUserPathways(data, successCallback, errorCallback)                                                 | Retrieves user pathways                                                         |
| ibledxpasswordreset | reset_password(successCallback)                                                                       | Resets password                                                                 |
| ibledxregister      | registerUser(data, successCallback, errorCallback)                                                    | Registers a user                                                                |
| ibledxtenants       | postTenantValidation(data, successCallback, errorCallback)                                            | Posts tenant validation                                                         |
| ibledxusers         | getUsersManageMetadata(data, successCallback, errorCallback)                                          | Retrieves users manage metadata                                                 |
|                     | postUsersManageMetadata(data, successCallback, errorCallback)                                         | Posts users manage metadata                                                     |
|                     | postUsersManage(data, successCallback, errorCallback)                                                 | Posts users manage                                                              |
|                     | forgotPassword(successCallback)                                                                       | Initiates forgot password process                                               |
|                     | getPublicDataByUsername(data, successCallback, errorCallback)                                         | Retrieves public data by username                                               |
|                     | postUserValidation(data, successCallback, errorCallback)                                              | Posts user validation                                                           |
|                     | postUserProfileImage(data, successCallback, errorCallback)                                            | Posts user profile image                                                        |
|                     | getUsersInfo(data, successCallback, errorCallback)                                                    | Retrieves users information                                                     |
|                     | patchUserFullname(data, successCallback, errorCallback)                                               | Updates user's full name                                                        |
|                     | patchUserEmail(data, successCallback, errorCallback)                                                  | Updates user's email                                                            |
|                     | getUserTenants(successCallback, errorCallback)                                                        | Gets user tenants                                                               |
| iblwebauth          | login(data, successCallback, errorCallback)                                                           | Logs in a user                                                                  |
|                     | completeLogin(successCallback)                                                                        | Completes the login process                                                     |
|                     | initializeLocalStorageWithAuthData(axdToken, dmToken, user, tenants, selectedTenant, successCallback) | Initializes local storage with authentication data                              |
|                     | registerUser(data, successCallback, errorCallback, param)                                             | Registers a new user                                                            |
|                     | forgetPassword(data, successCallback, errorCallback)                                                  | Initiates the forget password process                                           |
|                     | resetPassword(data, successCallback, errorCallback)                                                   | Resets the user's password                                                      |
| iblsearch           | orgSearch(data, successCallback)                                                                      | Performs organization search                                                    |
|                     | getCourseMeta(data, successCallback)                                                                  | Retrieves course metadata                                                       |
|                     | getOrgSkills(data, successCallback)                                                                   | Retrieves organization skills                                                   |
|                     | populateFilters(successCallback)                                                                      | Populates filters                                                               |
|                     | performSearch(fromInput, successCallback)                                                             | Performs search                                                                 |
| ibledxplatform      | updatePlatformConfig(data, successCallback)                                                           | Updates the platform configuration                                              |
|                     | sendEmail(data, successCallback)                                                                      | Sends an email                                                                  |
|                     | postTenant(data, successCallback, errorCallback)                                                      | Posts a new tenant                                                              |
|                     | getCsrfToken(expiryTime, successCallback, errorCallback)                                              | Gets the CSRF token                                                             |
