---
title: "How Should You Handle Form Validation with React Hook Form?"
date: 2021-07-03T01:42:26-04:00
description: "Considerations for how you define your validation rules"
tags:
  - react
  - react-hook-form
  - javascript
---

[React Hook Form](https://react-hook-form.com) is a great library for managing form state
in React. Getting started with it is simple, integrating with external libraries is (mostly)
seamless, and performance optimizations are baked into the library's core design. However, I want
to talk about validation in RHF.

In RHF, there are two main ways you define validation rules:

- Through passing a `rules` object to the individual fields (using the `register` or
  `Controller` APIs)
- Using a schema validation library like [Yup](https://github.com/jquense/yup)
  or [Zod](https://github.com/colinhacks/zod) at the root of the form

There isn't anything inherently wrong with either of these options, but both approaches
are not suitable for all use cases, and there are tradeoffs involved in each decision.

# Rules API

The Rules API built into React Hook Form is what you're given by default. It lets you pass in
rules on a **field level**. Using it with `register` would look something like:

```tsx
<input
  type="number"
  {...register("age", { required: true, min: 0, max: 150 })}
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

```tsx
const MySchema = Yup.object({
  age: Yup.number().required().min(0).max(150),
});

// then, within a component...
const methods = useForm({
  resolver: yupResolver(MySchema),
});
```

The above is example is equivalent to the one using the Rules API. Depending on which library you
choose, the built in rules can be more than sufficient. But of course, they also provide tools
for building your own if you need them.

# Which Should You Choose?

While both will let you define validation rules for your form, there are some questions you should
consider before making your decision.

## What Role Does Bundle Size Play?

When using Schema Validation, you will pull in at least two extra packages: the package for the
schema validation library and `@hookform/resolvers` (assuming you use the pre-built ones, you
can always build you own). This may be a problem for you, or the extra KiBs in your bundle are
worth it due to the benefits of using such a library.

## What Level Should the Rules Be Defined At?

Schema Validation forces you to define your rules up front _and then_ your actual fields, whereas
the Rules API is on a per-field basis. I've found that the Rules API is more flexible for scenarios
with many conditional fields because you only have to define the logic for conditional fields
once: in your JSX. For Schema Validation, you have to define the logic for that field both
in the JSX _and_ in the schema itself.

On the flip side, if you want to re-use a related group of fields with different validations
between the forms you use them in, Schema Validation will be more flexible. This is because the
form field definitions are separated from their rules, so you can "inject" whatever rules you
want into those fields for any number of re-usages.

## What is Convenient For Your Use Case?

Schema Validation libraries will typically have more rules built into them than React Hook Form's
Rules API will. Sure, you can always define your own rules, but that comes with its own mental
overhead, especially when trying to build a complex feature. You have a lot on your mind, and
trying to build a "is date in the past" rule, name it, and make it shareable to all of your other
forms doesn't reduce complexity.

---

While it may be tempting to choose one over the other for consistency's sake, I believe it is
better to choose whatever solves your current problem the best. So, use a mix of the two in your
app. I'd rather learn the two approaches but have an easier time modifying a form than
try to wade through a hacky usage of a tool that was forced to do something it wasn't designed
to do.
