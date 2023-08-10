# Next.js Client-Side Application Architecture for Forms

This client-side application architecture using Next.js streamlines the process of building forms by following an opinionated design pattern. This pattern is tailored to improve developer productivity and allow more focus on building the application faster. It even supports code generation by training an AI, adding another layer of efficiency to the development and maintenance process.

## Overview

The architecture consists of a few essential components that are used in the construction of forms. Whether it's a simple search bar or a complex input form, these components handle the state management, validations, and other necessities:

1. **Form Component:** Wraps the entire form, manages the form state, and provides common functionalities.
2. **Section Component:** Breaks the form into logical sections for better organization.
3. **Input Component:** Represents individual input fields, which can include various types like text, number, checkbox, etc.

### Creating a Form

Creating a form is straightforward with this architecture. Here's an example of a simple form containing a text input:

```jsx
import { Form, Section, Input } from "components/Form";

function MyForm() {
  return (
       <Form initialValue={initial_value} validationSchema={validation_schema} onSubmit={onSubmit}>
       <Section title="Basic Information">
           <Input name="address" label="Address" type="input" />
           <Input name="state" label="State"  key="states" type="dropdown" function={() ⇒ {
               return fetch("/api/fetch/").response(data ⇒ data.json())
            }} />
           <Input name="reporting_district" label="District"
            function={(values) ⇒ {
               return fetch("/api/fetch/" + value.state).response(data ⇒ data.json())
            }}  key="reporting_district" type="dropdown" />
       </Section>
    </Form>

  );
}
```

## Component Documentation

### Form Component

The `Form` component is the root container for a form. It provides form-level state management and common functionalities.

#### Props

- `onSubmit`: Function to handle form submission.
- `initialValue`: Object that defines the initial values for the form fields.
- `validationSchema`: (Optional) Object that defines the validation schema for the form.

#### Example

```jsx
<Form
  onSubmit={handleSubmit}
  initialValue={initialValues}
  validationSchema={validationSchema}
>
  {/* Form content */}
</Form>
```

### Section Component

The `Section` component organizes the form into logical sections to improve readability and maintainability.

#### Props

- `title`: String that defines the title of the section.

#### Example

```jsx
<Section title="Personal Information">{/* Section content */}</Section>
```

### Input Component

The `Input` component represents individual input fields within the form. It handles various input types, including text, number, checkbox, and more.

#### Props

- `type`: String that defines the input type (e.g., "text", "number", "checkbox", etc.).
- `name`: String that specifies the name attribute for the input.
- `label`: String that defines the label for the input.
- `placeholder`: (Optional) String for the input placeholder.
- `key`: This will be a unique key which will cache the results from the function.
- `function`: If fetching data is required for the component to operate.
- `cache`: (Optinal) If the data should be cached for future. If True the function will only be called if the cache for the given key is empty.
  - Default: True
- `fetchType`: (Optional) This will define how the function method will be called.
  - `onMount`: (Default) The function will be called when the coponent mounts.
  - `onTouch`: The function will be called when the component is touched by the user.
  - `onChange`: The function will be called at the end of the user interaction of a component.
  - `onBlur`: The function will be called at the end of the user interaction of a component.

#### Example

```jsx
<Input type="text" name="name" label="Name" placeholder="Enter your name" />
```

## Data Fetching use cases.

Here are some data fetching use cases to show the architecure in action.

1. Fetching the states to populate a dropdown. (Given that the `country is same`)

#### Example

```jsx
<Input type="dropdown" name="states" label="States" key="states" function={() ⇒ {
    return fetch("/api/states").response(data ⇒ data.json())
}} />

```

#### Explanantion

This example showcase how a dropdown for states would look like.
We have a function call to fetch data to populate this dropdown. Since we have a pretty good idea that the states won't change during the application lifecycle, it's safe to fetch it once and cache in the client side so we leave `cache` as `True`. Since we have a function defined by default the component is going to assume that the `fetchType` is `onMount`, so when the component mounts the fetch request will be triggerd and since cache is True if the cache is empty for this key it will trigger the function.

2. Fetching the states to populate a dropdown. (Given that the `country changes`)

#### Example

```jsx

<Input type="dropdown" name="states" label="States" key="states" function={() ⇒ {
    return fetch("/api/states").response(data ⇒ data.json())
}} />

```

### State Management

State management is handled within the components themselves, so you can focus more on building the application.

### Code Generation AI Integration

The architecture supports a code generation AI that can be trained to output code in the project's specific format. This can drastically reduce the time needed to build new features.

## Contribution Guidelines

We welcome contributions. Before making any changes, please read our [CONTRIBUTING.md](./CONTRIBUTING.md) file.

## Support and Contact

If you have any questions or need support, please contact us at support@your-organization.com.

## License

This project is licensed under the MIT License. See the [LICENSE](./LICENSE) file for details.

---

By following this opinionated design pattern, you'll find building forms in your Next.js application to be a more cohesive and efficient experience. Happy coding!
