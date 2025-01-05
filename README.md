# forSead
RBAC information


## Prompt

I have an ecommerce website where I have a bunch of Teacher Manuals available to subscribers or end users. We have three different programs that have manuals for various grade levels, please see below:

Program1 is Sirah - Grades 1-6
Program 2 is Aqeedah/Fiqh/Ikhlaq - Grades 1-6
Program 3 is Weekend Schools - Grades 1-2

What I want to be able to do is provide teachers access to specific content, by program as well specific grade levels, if they want access to all they pay a different fee, or if they want access to one specific grade they pay for that material only.

I have a wordpress site currently and am redesigning the website to incorporate a login and allow teachers to access their paid for content only. 

How can I do what I want to do?

## Response

To support your requirements for providing specific access to different programs and grade levels on your e-commerce WordPress site, you will need a user data model that takes into account user roles, access levels, and subscription plans. Here's a conceptual overview of how you might structure the user data model:

### WordPress User Data Model Structure:

1. **User Roles**:
   - **Subscriber**: Default role for teachers who sign up for the website.
   - **Member/Teacher**: Enhanced role given to those who purchase any subscription plan. This role can be created via membership plugin functionalities and would include:
     - Ability to access purchased content.
     - Access to a dashboard to view available materials and subscriptions.

2. **Custom User Metadata**:
   - Use custom fields (user meta) to store additional details about subscriptions. Most membership plugins will handle this automatically:
     - `subscription_plan`: Identifier for the user's subscribed plan (e.g., `sirah_grade_3`, `weekend_schools_grade_1`).
     - `access_level`: Summary of access rights (e.g., `program1_grade_3`).
     - `subscription_status`: Active, inactive, canceled.
     - `subscription_start_date`: Record of when the subscription started.
     - `subscription_end_date`: If applicable, the end date of access.
   
3. **Content Access Mapping**:
   - Each piece of content (teacher manuals, videos, etc.) should have metadata that maps it to the appropriate subscription plan:
     - `accessible_to`: List of subscription plans that grant access (e.g., `sirah_grade_1`).
   
4. **Subscription and Payment Details**:
   - Stored in association with the user, typically in user meta or via the membership plugin:
     - `payment_method`: Last used payment method information, often tokenized/encrypted.
     - `billing_details`: If stored, includes address, name on card, etc.
     - `transaction_history`: Log of past payments, often managed by the plugin.
   
5. **Additional Custom Fields** (if needed):
   - For added functionality or reporting, add fields like:
     - `last_login`: Timestamp of the user's last login.
     - `last_accessed_content`: The ID or name of the last content the user accessed.
     - `preferred_contact_method`: Email, phone, etc., if you collect this info.

### Implementation Consideration:

- **Membership Plugin API**: Most membership plugins like MemberPress or Restrict Content Pro come with APIs to interact with user metadata when needed, extending the ability to query and manipulate user subscription data easily.
   
- **Custom Code for Flexibility**: Use custom PHP functions or hooks to extend or modify the behavior of membership plugins as necessary, such as adding custom fields or changing roles upon subscription events.

- **User Experience**: From a UX standpoint, ensure that wherever users log in, they clearly see what content and subscriptions are available to them, and make it easy to navigate or upgrade/downgrade their subscription.

### Summary:

The combination of WordPress roles, membership plugin capabilities, and custom user meta will help create a flexible and robust user data model. This model will facilitate the controlled access to your courses based on your specified content access rules. Leveraging a membership plugin will abstract many complexities as they come pre-built with functionalities that cater to such requirements.

Below is a table format that outlines the conceptual user data model for your WordPress site with user roles, metadata, and content access considerations.

| **User Data Model Element** | **Description** | **Example Values / Notes** |
|-----------------------------|-----------------|----------------------------|
| **User Roles**              | User roles define access level and permissions. | - **Subscriber**: Default role for newly registered users.<br>- **Member/Teacher**: Upgraded role on subscription purchase. |
| **Custom User Metadata**    | Custom fields to capture additional subscription details. | |
| `subscription_plan`         | Identifier for the subscribed plan. | `sirah_grade_3`, `aqeedah_fiqh_ikhlaq_grade_2`, `weekend_schools_grade_1` |
| `access_level`              | Summary of content access rights. | `program1_grade_3`, `full_program_access` |
| `subscription_status`       | Status of the user's subscription. | `active`, `inactive`, `canceled` |
| `subscription_start_date`   | Date when the subscription started. | `2023-01-15` |
| `subscription_end_date`     | Date when subscription ends, if applicable. | `2024-01-14` |
| **Content Access Mapping**  | Maps content to the appropriate subscription plans. | |
| `accessible_to`             | List of subscription plans that grant access to a piece of content. | `sirah_grade_1`, `full_program_access` |
| **Subscription and Payment Details** | Financial and transaction details linked to a user. | |
| `payment_method`            | Last used payment method (tokenized/encrypted). | `stripe_card`, `paypal` |
| `billing_details`           | User's billing information, if stored. | Address, name on card |
| `transaction_history`       | Log of past payments, transactions. | Automatically managed by the plugin. |
| **Additional Custom Fields** | Optional fields for extended functionality or reporting. | |
| `last_login`                | Timestamp of the user's last login. | `2023-10-05 14:35` |
| `last_accessed_content`     | The ID or name of the last accessed content. | `sirah_class_3_manual.pdf` |
| `preferred_contact_method`  | User's preferred contact method. | `email`, `phone` |

### Key Considerations:

- **User Roles**: Setting the correct role is crucial as it determines what subscription plans a user can have and what content they can access.
- **Custom User Metadata**: Use the functionality of a membership plugin to enhance user profiles with these fields.
- **Content Access Mapping**: Ensure that each manual or resource on your site is correctly tagged with the subscriber plans that can access it.
- **Subscription and Payment Details**: Handling of sensitive information should comply with relevant regulations (e.g., GDPR, PCI DSS).

Organizing user data in this manner supports a scalable, efficient system for managing access to your educational content on your WordPress site.


## Modified User Data Model

To modify user roles to be based on program type and list all possible options for roles and subscriptions, you'll have a more granular level of control over what content a user can access. Below is the updated table that reflects user roles based on program types and outlines possible role and subscription options.

### User Roles and Subscription Options

| **Element**                  | **Description**                                                                                  | **Example Values / Notes**                                |
|------------------------------|--------------------------------------------------------------------------------------------------|-----------------------------------------------------------|
| **User Roles**               | Define roles based on specific program types to control access levels.                          |                                                           |
| `Sirah_User`                 | Role assigned to users subscribed to any plan within the Sirah program.                         | Access to Sirah-related materials.                        |
| `Aqeedah_Fiqh_Ikhlaq_User`   | Role assigned to users subscribed to the Aqeedah/Fiqh/Ikhlaq program.                           | Access to Aqeedah, Fiqh, and Ikhlaq materials.            |
| `Weekend_School_User`        | Role for users subscribed to the Weekend Schools program, especially Grades 1-2.                | Access to Weekend School materials.                       |
| `All_Programs_User`          | Role for users who subscribe to access all programs and grades.                                 | Comprehensive access to all content from all programs.    |
| **Subscription Plans**       | Detailed options for subscription plans available to users, categorized by program and grade.   |                                                           |
| `sirah_grade_1`              | Subscription for Sirah program, Grade 1.                                                        |                                                           |
| `sirah_grade_2`              | Subscription for Sirah program, Grade 2.                                                        |                                                           |
| `sirah_grade_3`              | Subscription for Sirah program, Grade 3.                                                        |                                                           |
| `sirah_grade_4`              | Subscription for Sirah program, Grade 4.                                                        |                                                           |
| `sirah_grade_5`              | Subscription for Sirah program, Grade 5.                                                        |                                                           |
| `sirah_grade_6`              | Subscription for Sirah program, Grade 6.                                                        |                                                           |
| `aqeedah_fiqh_ikhlaq_grade_1`| Subscription for Aqeedah/Fiqh/Ikhlaq program, Grade 1.                                           |                                                           |
| `aqeedah_fiqh_ikhlaq_grade_2`| Subscription for Aqeedah/Fiqh/Ikhlaq program, Grade 2.                                           |                                                           |
| `aqeedah_fiqh_ikhlaq_grade_3`| Subscription for Aqeedah/Fiqh/Ikhlaq program, Grade 3.                                           |                                                           |
| `aqeedah_fiqh_ikhlaq_grade_4`| Subscription for Aqeedah/Fiqh/Ikhlaq program, Grade 4.                                           |                                                           |
| `aqeedah_fiqh_ikhlaq_grade_5`| Subscription for Aqeedah/Fiqh/Ikhlaq program, Grade 5.                                           |                                                           |
| `aqeedah_fiqh_ikhlaq_grade_6`| Subscription for Aqeedah/Fiqh/Ikhlaq program, Grade 6.                                           |                                                           |
| `weekend_school_grade_1`     | Subscription for Weekend Schools program, Grade 1.                                              |                                                           |
| `weekend_school_grade_2`     | Subscription for Weekend Schools program, Grade 2.                                              |                                                           |
| `all_programs_access`        | Comprehensive subscription that grants access to all programs and all grades.                   |                                                           |

### Implementation Notes:

- **Role Assignment**: Users should be assigned a role based on the highest level of access within their chosen program. For example, if a user subscribes to `sirah_grade_3`, they become a `Sirah_User`.
- **Subscription Changes**: When users upgrade or modify their plan, ensure their role reflects the new highest level of access applicable.
- **Control Access**: Use the roles to control access to content through your membership plugin or custom access control settings.

This structure allows for clear segmentation of roles and subscription plans, making it easier to manage who can see what content on your WordPress site.
