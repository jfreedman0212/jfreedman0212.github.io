---
title: "How Should You Handle Validation Rules with React Hook Form?"
date: 2021-07-03T01:42:26-04:00
description: "Considerations for how you define your validation rules"
tags:
  - react
  - react-hook-form
  - javascript
---

[React Hook Form](https://react-hook-form.com) is a great library for managing form state
in React. Getting started with it is simple, integrating with external libraries is (mostly)
seamless, and performance optimizations are baked into the library's core design. Like any other
good form library, it comes with tools for validating user input. RHF provides you two choices that might seem equivalent, but have different strengths/tradeoffs.

# Rules API

The Rules API built into React Hook Form is what you're given by default. It lets you pass in
rules on a **field level**. Using it with `register` would look something like:

```tsx
<input
  type="number"
  {...register("age", {
    required: true,
    min: 0,
    max: 150
  })}
/>
```

As you probably guessed, the above input is required and must be at least 0 but no more than 150.
This method has a couple more built in options, some data transformation options, and a fallback
`validate` function/set of functions that you can define yourself.

# Schema Validation Libraries

Schema Validation libraries typically provide tools for building "schemas" that validate an
object you pass to it passes all of the rules associated with the schema. Since RHF maps your
data to a JS object, you can pass that data to a schema to perform validation. There are many
libraries you can choose from for this, but I'll give an example of it with Yup:

```ts
const MySchema = Yup.object({
  age: Yup.number()
    .required()
    .min(0)
    .max(150)
});

// then, within a component...
const methods = useForm({
  resolver: yupResolver(MySchema),
});
```

The above example is equivalent to the one using the Rules API. Depending on which library you
choose, the built in rules can be more than sufficient. But of course, they also provide tools
for building your own if you need them.

# Which Should You Choose?

While both will let you define validation rules for your form, there are some considerations to think about before coming to a decision (ordered by importance).

## Level of Control

When using the Rules API, you define your validation rules right on the field itself. Conversely,
you define your rules at the top of the form when using a Schema Validation library.

I've found that the Rules API is more flexible for scenarios with many conditional fields 
because you only have to define the logic for conditional fields
once: in your JSX. For Schema Validation, you have to define the logic for that field both
in the JSX _and_ in the schema itself. On the flip side, if you want to re-use a related group of fields with different validations
between the forms you use them in, Schema Validation will be more flexible. This is because the
form field definitions are separated from their rules, so you can "inject" whatever rules you
want into those fields for any number of re-usages.

This may change between forms in your application, so adopting both approaches based on
the requirements may be best (as opposed to choosing one for _all_ forms).

## Built In Rules

Schema Validation libraries will typically have more rules built into them than React Hook Form's
Rules API will. Sure, you can always define your own rules, but then you have to write the code to validate the field,
figure out how to share this rule for other places in your app, maintain it, and make many other decisions that distract
from the feature you're just trying to add. Or, you could just call `Yup.string().email()` and be done with it. 

## Bundle Size

When using Schema Validation, you will pull in at least two extra packages: the package for the
schema validation library and `@hookform/resolvers` (assuming you use the pre-built ones, you
can always build you own). This will increase the size of the code being shipped (as bringing
in any packages will).

---

As with every other decision, weigh the benefits of each approach with the tradeoffs for your 
use case. The end goal of every decision is to make it as easy as possible to build and maintain.
