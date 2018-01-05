---
layout: default
title:  algorithmic life, part i
excerpt: deodorant or moisturizer
categories: nonsense
---

## algorithmic life part 1

the question at hand is this: when one only has time to complete _either_ applying deodorant _or_ applying moisturizer to the face, which does one choose? 

```javascript
const DEODORANT = 'deodorant'
const MOISTURIZER = 'moisturizer'

function deodorantOrMoisturizer() {
  // return DEODORANT ?
  // return MOISTURIZER ?
}
```

we clearly need more information to make the proper decision. for convenience, we will refer to our target entity as *Ashley*, with sex indeterminate, for the rest of this inquiry.

starting as close to the surface as possible, we need to know if the face needs moisturizer at this time. if it doesn't, the answer seems clear. also of note is that if Ashley is going out in public later, we would want to emphasize deodorant for the sake of many, although this rule does come with the proviso that if Ashley's face is so drastically dry as to cause both discomfort to Ashley *and* others, it would be prudent to elect moisturizer instead.

so we will need to keep track of these branches with variables. we will need to know specific properties of both Ashley and the current state of life. future plans for Ashley would be part of the current state of life, as the plans are made now, not then.

```javascript
const DEODORANT = 'deodorant'
const MOISTURIZER = 'moisturizer'

function deodorantOrMoisturizer (person, state) {
  // extract the variables of importance from person and state
  let { dryFace } = person
  let { goingOut } = state

  if (goingOut) {
    if (dryFace.severity >= 8) { // severity 1-10
      return MOISTURIZER
    }
    return DEODORANT
  }
  // not going out but face is not dry
  if (!dryFace) {
    return DEODORANT
  }
  return MOISTURIZER
}
```

now there exists, of course, a demon in this angelic clarity, and that is Ashley's opinion. it could very well be that Ashley chooses definitively one or the other and the algorithm must comply. it could also be that Ashley is not very opinionated, in general, or specifically about this question. these opinions would also include any religious beliefs concerning deodorant or moisturizer, although Ashley might think of them more as incontrovertible truths than as opinions. these cases need to be considered by updating our person object.

```javascript
const DEODORANT = 'deodorant'
const MOISTURIZER = 'moisturizer'

function deodorantOrMoisturizer (person, state) {
  // extract the variables of importance from person and state
  let { opinions, dryFace } = person
  let { goingOut } = state

  // check if person has opinions, and specifically opinions about this issue
  if (opinions.activated && (opinions.about(DEODORANT) || opinions.about(MOISTURIZER))) {
    // opinions.about(x) returns 1-10 importance or 0 if no opinion
    if (opinions.about(DEODORANT) > opinions.about(MOISTURIZER)) {
      return DEODORANT
    } else if (opinions.about(MOISTURIZER) > opinions.about(DEODORANT)) {
      // we need this explicit branch to avoid returning
      // in the case of a tied opinion (which is equal to no opinion)
      return MOISTURIZER
    }
  }

  if (goingOut) {
    if (dryFace.severity >= 8) { // severity 1-10
      return MOISTURIZER
    }
    return DEODORANT
  }
  // not going out but face is not dry
  if (!dryFace) {
    return DEODORANT
  }
  return MOISTURIZER
}
```

we see from this that we have arrived at an interesting, albeit possible incorrect, fact: moisturizing is the default option, in the absence of all other variables. if Ashley is not going out, does not have an already moist face, and does not have strong opinions about deodorant vs. moisturizer, Ashley ought to just moisturize.

there is the possibility that Ashley is incapable of moisturizing or deodorizing, due to allergies, dismemberment, incapacitation, etc. we will bundle all of this into positive variables `canMoisturize` and `canDeodorize`:

```javascript
const DEODORANT = 'deodorant'
const MOISTURIZER = 'moisturizer'

function deodorantOrMoisturizer (person, state) {
  // extract the variables of importance from person and state
  let { opinions, abilities, dryFace } = person
  let { goingOut } = state

  // check abilities first
  if (!abilities.canMoisturize) {
    return DEODORANT
  } else if (!abilities.canDeodorize) {
    return MOISTURIZER
  }

  // check if person has opinions, and specifically opinions about this issue
  if (opinions.activated && (opinions.about(DEODORANT) || opinions.about(MOISTURIZER))) {
    // opinions.about(x) returns 1-10 importance or 0 if no opinion
    if (opinions.about(DEODORANT) > opinions.about(MOISTURIZER)) {
      return DEODORANT
    } else if (opinions.about(MOISTURIZER) > opinions.about(DEODORANT)) {
      // we need this explicit branch to avoid returning in the case of a tied opinion (which is equal to no opinion)
      return MOISTURIZER
    }
  }

  if (goingOut) {
    if (dryFace.severity >= 8) { // severity 1-10
      return MOISTURIZER
    }
    return DEODORANT
  }
  // not going out but face is not dry
  if (!dryFace) {
    return DEODORANT
  }
  return MOISTURIZER
}
```

this should provide a sufficent starting point upon which progress can be made.