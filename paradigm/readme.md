# Paradigm

When a relation model instance satisfied some logical rules, we say it satisfy some _paradigm_ .

In this part we are going to learn about 5 different sequential paradigm.

- 1NF
- 2NF
- 3NF
- BCNF
- 4NF

Any __relation schema that satisfy lower paradigm will also satisfy the paradigm above__. For example, if a relation satisfy `3NF`, then it must also satisfy `1NF` and `2NF`.

# Funtional Dependency

## Basic Definition

In a relation, __column set__ `x` and `y` belongs to this relation.

If: Can NOT find two rows that `x` values equal, but `y` values not equal.

Then we say:

- `x -> y`. Or `y` is __functional depend__ on `x`.
- `x` is the __determinant__ of this dependency.

## Trivial / Non-Trivial Dependency

$$
x \to y, x \supseteq y
$$

This is __Trivial Functional Dependency__, for any relation, _Trivial Functional Dependency_ will always be satisfied.

$$
x \to y, x \nsupseteq y
$$

Correspondingly, this is __Non-Trivial Functional Dependency__, expect explicit declaration, all dependency we talked below refer to this one.

## Full dependency

If $x \to y$, and any $x' \subseteq x$ can not satisfy $x' \to y$, then we say `y` is fully depend on `x`. Otherwise, we say `y` is partially depend on `x`.

## Transmissive Dependency

If `x->y`, `y!->x`, `y->z`. Then `x->z`. We say `z` is transmissively depend on `x`.

## Key

Here if we have relation `R`. if __column set__ X, `X ->(full) R`, then `X` is a key. Any column in `X` is key attr.

# 1NF

This is the most simple and basic one. Any relation with non-diviable column satisfy `1NF`.

# 2NF

- Satisfy `1NF`
- Any non-key-attribute column are fully depend on any key.

## Example

```
- Student lives in same dormitory if they have same major
- Student could select more than one course.

Table(stu_id, major_id, dorm_id, course_id)

Candidate Key: (stu_id, course_id)
Primary Attrs: stu_id, course_id
Dependency Status:
- primary_key -> R
- stu_id -> major_id
- major_id -> dorm_id
- stu_id -> dorm_id
```

![IMG_2148](https://github.com/Oya-Learning-Notes/SQL-Learning-Note/assets/61616918/050ae975-ae43-4262-b8bd-c7dbbdddf07a)

Notice in the example above, `major_id`, `dorm_id` are depend on `stu_id` which means they are _partially depend on primary key_. This break `2NF` rules, so this relation not satisfy `2NF`.

To make is satisfy `2NF` we can divide the relation:

```
Table1(stu_id, major_id, dorm_id)
Table2(stu_id, course_id)

Table2 is obviously satisfy 2NF

Table1:
Key: (stu_id) 
Depend:
stu_id -> major_id
stu_id -> dorm_id
major_id -> dorm_id

Any non-key-attr(here is major_id and dorm_id) are fully depend on stu_id
```

![IMG_2149](https://github.com/Oya-Learning-Notes/SQL-Learning-Note/assets/61616918/79f8f695-68a5-4887-ac35-bf080bdfac35)


## Drawback

- __Insertion Exception__ In example above, we can not insert student that have not selected any course.
- __Deletion Exception__ If a student only select one course, then we can deselect this course for this student.
- __Update Complexity__ The data is duplicated, and it will be difficult if we want to update the major of a student. (All record related to this student need to be update, and the `dorm_id` also need to be update)

# 3NF

Not exist key `X`, attr `Y`, non-key-attr `Z` that satisfy `X->Y, Y!->X, Y->Z`.

- Satisfy 2NF
    - Satisfy 1NF
    - Any non-key-attrs are fully depend on candidate key.
- Any non-key-attrs are not transmissively depend on candidate key.

## Example

We use the revised `2NF` example:

```
Table1(stu_id, major_id, dorm_id)
Table2(stu_id, course_id)
```

![IMG_2149](https://github.com/Oya-Learning-Notes/SQL-Learning-Note/assets/61616918/79f8f695-68a5-4887-ac35-bf080bdfac35)

Notice that although it satisfy `2NF`, but in _Table1_, we have:

```
stu_id -> major_id -> dorm_id

Transmissive Dep, 3NF rules breaked
```

To convert this relation into a `3NF` relation, we need to again divide the table.

```
StudentCourse(stu_id, course_id)
StudentMajor(stu_id, major_id)
StudentDorm(stu_id, dorm_id)
```

The relation above satisfys `3NF` (Need to be scrutinize)

# BCNF

`BCNF` is similar to `3NF`, but don't allow transmissive deps even in primary attributes.

That's, all `x -> y`, `x` must include key.

# Conclusions:

- `1NF` Basic.
- `2NF` All attributes fully depend on any `candidate key`.
- `3NF` Not exists key `X`, attr set `Y`, non-primary attr `Z` that satisfy `X -> Y, Y -> Z`.
- `BCNF` Any `X -> Y, Y != X`, `X` must includes `candidate key`.