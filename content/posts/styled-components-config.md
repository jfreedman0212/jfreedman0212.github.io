---
title: "Configuring Nested Styled Components"
date: 2021-07-03T00:42:26-04:00
description: "Fine-grained and localized styling for Styled Components"
tags:
  - react
  - styled-components
  - css
  - javascript
---

Let's consider a component that looks something like this (ignoring the obvious issues with the form itself):

```tsx
const Form = styled.form`
    padding: 1rem;
    border: 1px solid black;
    display: flex;
    flex-direction: column;
    gap: 2rem;
`;

const Input = styled.input`
    border-radius: 5px;
    padding: 1rem;
`;

const InformationForm: FC = () => {
  return (
      <Form>
        <Input name="firstName" />
        <Input name="lastName" />
      </Form>
    );
};

export default InformationForm;
```

I build out my feature like this and I'm on my way. Later down the line, we
want to add this form to another app with some minor tweaks:
- The fields need to laid out horizontally instead of vertically
- The form needs to be a simple gray box with no border

We could pull this out into some global configuration/theming, but that seems too heavy
for something as localized as this. Instead, we could expose the individual Styled 
Components and extend them how we see fit. At the bottom of the code above, we add:

```tsx
export default InformationForm;
export { Form };
```

And in the new file where we're making the new version:

```tsx
import InformationForm, { Form } from "./InformationForm";

const NewStyles = styled.div`
    & ${Form} {
        flex-direction: row;
        border: none;
        background-color: #eee;
    }
`;

const NewInformationForm: FC = () => {
    return (
        <NewStyles>
            <InformationForm />
        </NewStyles>
    );
};
```

# Why Is This Better?

The problem of "I have this component but just need to tweak _one_ style" comes up
pretty often, especially when trying to share components between applications. Your
stateful logic is the same, so making more components seems like a weird way to
go about this. There are two other alternatives I can think of:

1. Move these styles up into the top-level theme (as mentioned before)
2. Add props to my component that dictate these details

Moving it into the theme _may_ make sense, but I believe that this isn't the best choice.
This would allow other components to base their styles off of that one. Why should your 
nav bar layout care how your "user information" form is laid out? Instead, the
top-level theme should be where your "theme primitives" go (brand colors, spacing, etc.).

Adding props for this _may also_ make sense. Sometimes, this may be the way to go! I 
would go for this approach if the number of options for that prop are finite. In the
example above, we could add a `direction` prop to `InformationForm` that takes in
either `"row"` or `"col"` and switches between the appropriate values for `flex-direction`.
However, for cases where we either need total control or the options are not finite/large,
this can become cumbersome.

As always, there is no single "correct" approach, so use your best judgement.