---
title: Breaking The Chain
---

## Intro

Explanation of the problem & why you should care.

## Promise Chain 

```javascript
let iWillTurnInMyTermPaper = beAssignedTermPaper()
    .then(instructions => {
        return doResearch(instructions);
    })
    .then(research => {
        return writePaper(research);
    })
    .then(paperWritingResults => {
        return doExtraCredit(paperWritingResults);
    })
    .then(assignments => {
        return turnInAssignment(assignments);
    });
```

```javascript
function beAssignedTermPaper() {
    return {
        topic: "Lettuce Sea Slug",
        description: "Write no less than 500 words on the habitat and behaviors of the Lettuce Sea Slug"
        wordCount: 500,
        extraCredit: "Include a picture of the beastie"
    };
}

function doResearch(instructions) {
    let gloriousInsights = ponder(instructions.description);
    let actualResearch = askGoogle(instructions.topic);
    return {
        wordCount: instructions.wordCount,
        contents: gloriousInsights + actualResearch
        extraCreditInstructions: instructions.extraCredit
    }
}

function writePaper(research) {
    return {
        researchPaper: research.contents * research.wordCount,
        extraCreditInstructions: instructions.extraCreditInstructions
    }
}

function doExtraCredit(paperWritingResults) {
    let extraCredit = use(paperWritingResults.extraCreditInstructions);
    return [ paperWritingResults.researchPaper, extraCredit ];
}

function turnInAssignment(assignments) {
    return assignments.printAndStaple();
}
```

Kind of yucky, right? Circuitous, inefficient, we need to pass parameters through multiple functions, and we are constrained to performing tasks in a specific order. There's no reason you have to write the paper befor doing the extra credit. You could probably even do those tasks in parallel. There must be a better way!  

## Break The Chain 

There's totally a better way. Check it out:

// maybe put function definitions first? 
// not sure how much value the function definitions add to the main argument

```javascript
let iWillBeAssignedATermPaper = beAssignedTermPaper();

let iWillResearchMyTopic = iWillTurnInMyTermPaper.then(instructions => {
    return doResearch(instructions);
});

let iWillWriteMyPaper = iWillResearchMyTopic.then(research => {
    return writePaper(research);
});

let iWillDoTheExtraCredit = iWillBeAssignedATermPaper.then(instructions => {
    return doExtraCredit(instructions);
});

let iWillTurnInMyTermPaper = Promise.all([iWillWriteMyPaper, iWillDoTheExtraCredit]).then(assignments => {
    let paper = assignments[0].printAndStaple();
    let picture = assignments[1].printOnGlossyPaper();
    return paper + picture;
});
```

And we can rewrite our function like so ...

```javascript
function beAssignedTermPaper() {
    return {
        topic: "Lettuce Sea Slug",
        description: "Write no less than 500 words on the habitat and behaviors of the Lettuce Sea Slug"
        wordCount: 500,
        extraCredit: "Include a picture of the beastie"
    };
}

function doResearch(instructions) {
    let gloriousInsights = ponder(instructions.description);
    let actualResearch = askGoogle(instructions.topic);
    return {
        wordCount: instructions.wordCount,
        contents: gloriousInsights + actualResearch
        extraCreditInstructions: instructions.extraCredit
    }
}

function writePaper(research) {
    return {
        researchPaper: research.contents * research.wordCount,
        extraCreditInstructions: instructions.extraCreditInstructions
    }
}

function doExtraCredit(paperWritingResults) {
    let extraCredit = use(paperWritingResults.extraCreditInstructions);
    return [ paperWritingResults.researchPaper, extraCredit ];
}

function turnInAssignment(assignments) {
    return assignments.printAndStaple();
}

```

Ahh, much cleaner.

## Add A Fail Case 

What happens if, through no fault of your own, you develop a severe allergy to homework assignment while in the middle of `writePaper`. Well then you'll need to reject the entire promise chain! Here's how you might do that:

## No-frills example 

```javascript
```

## Conclusion

Reiterate the problem & why you should care using the absurd use cases from the examples