```{=latex}
\usepackage{hyperref}
\usepackage{graphicx}
\usepackage{listings}
\usepackage{textcomp}
\usepackage{fancyvrb}

\newcommand{\passthrough}[1]{\lstset{mathescape=false}#1\lstset{mathescape=true}}
\newcommand{\tightlist}{}
```

```{=latex}
\title{CAST Incident Retrospectives}
\author{Moshe Zadka -- https://cobordism.com}
\date{}

\begin{document}
\begin{titlepage}
\maketitle
\end{titlepage}

\frame{\titlepage}
```

```{=latex}
\begin{frame}
\frametitle{Acknowledgement of Country}

Belmont (in San Francisco Bay Area Peninsula)

Ancestral homeland of the Ramaytush Ohlone people

\end{frame}
```

I live in Belmont,
in the San Francisco Bay Area Peninsula.
I wish to acknowledge it as the
ancestral homeland
of the
Ramaytush Ohlone people.

## Basics

### What is an incident?

```{=latex}
\begin{frame}
\frametitle{What is an Incident?}

Bad\pause

Unplanned\pause

Willing to stop

\end{frame}
```

An incident is something that is:

* Bad: If it wasn't a problem, there's no issue.
* Unplanned: If the problem was *planned*, it is not an incident.
  This is not the same as saying the problem was
  *caused*
  by a planned thing.
  If a website was planned to shut down for three hours for an upgrade,
  and it shut down,
  this is fine.
  If an upgrade caused a three hour outage,
  it might be an incident.
* Willing to stop: An incident has to be something that,
  at least in principle,
  we are willing to put effort into stopping.
  If an earthquake took out our datacenter,
  and the risk of earthquakes is low,
  we might not be willing to put in the effort to stop it.

### What is an incident retrospective?

```{=latex}
\begin{frame}
\frametitle{What is an Incident Retrospective?}

(Post-mortem)\pause

Analyze\pause

Improve

\end{frame}
```

An incident retrospective is a process that allows us to analyze the incident
and improve our future resilience based on the analysis.
The process usually has both an async part,
where someone does the research, analysis, and suggested improvement,
and a synchronous part,
where the analysis is validated
and the team commits to the improvement plan.

### Why is an incident retrospective?

```{=latex}
\begin{frame}
\frametitle{Why is an Incident Retrospective?}

\pause
Do better!

\end{frame}
```

We do incident retrospectives because an
incident
teaches us about a lack of resiliency in our system.
We want our systems to be more resilient.
The
"natural lesson"
of an incident is a poor thing to waste.

### What is root cause analysis?

```{=latex}
\begin{frame}
\frametitle{What is root cause analysis?}

Find cause...\pause
eliminate!


\end{frame}
```

Root Cause Analysis,
sometimes in the form of the
"five whys",
asks
"what originally caused the problem?"
The goal of an RCA
is to
*identify*
the root cause,
and then eliminate it.

Note that this is a crucial piece of
RCA:
the improvement is to
*eliminate the root cause*.
The theory underlying it is that
any other step along the way
would not have happened
without the original problem.

### Why not root cause analysis?

```{=latex}
\begin{frame}
\frametitle{Why not root cause analysis?}

More than one\pause

Too specific\pause

Not controllable

\end{frame}
```

Why not do RCA-based analysis?

For one,
there might be
*more than one*
*root*
cause.
In some situations,
*two*
things had to have happened:
without either,
the incident would not happen.

The root cause might also be
*too specific*.
If we eliminate the root cause,
but another one can start the same chain of events,
we have fixed the
*wrong problem*.

Finally,
the root cause might be
*beyond our boundary of control*.
If the root cause is
"high customer traffic following a world-wide event",
then unless we control the world,
we cannot remove the cause.

Sometimes,
RCA has morphed into
"contributing cause analysis".
This tries to address the issue
by suggesting we look at
"all"
things that caused the incident.
This is a patch on RCA
which transforms a poor system
to a poorly defined system.

Instead,
the right way to fix the issue is to go back to the drawing board:
how do we make systems more resilient?
There is a field that deals with those issues:
systems theory.

## System Theory Basics

### System

```{=latex}
\begin{frame}
\frametitle{System}

Components\pause

You control

\end{frame}
```

Systems theory,
like any field,
has some technical jargon.
These are specific terms
with precise meaning,
different than their meaning in regular use.

A
*system*
is a collection of
*components*.
A component is only in the system
if
*you*
control it.

This means,
for example,
your co-lo datacenter's power is
*not*
part of your system.


### Environment

```{=latex}
\begin{frame}
\frametitle{Environment}

Influences behavior\pause

Not system

\end{frame}
```

The *environment*
is anything that influences the behavior of the system
but is not a part of it.
This can include the people who use the system,
the underlying network infrastructure,
or even the weather!

### Control

```{=latex}
\begin{frame}
\frametitle{Control}

Input\pause

to output

\end{frame}
```

A
*control*
is a part of the component.
It is something that takes input,
from the system or environment,
or produces some result or output.

An example of a control
is a thermostat.
The input is a temprature,
and the output is to connect
or disconnect an electric circuit.

### Loss

```{=latex}
\begin{frame}
\frametitle{Loss}

Undesired behavior

\end{frame}
```

A system has things it should do.
When it does not do these things,
the technical term is
"loss".

A loss can be
"our website was unavailable for five seconds,
so people could not see cat pictures"
or
"we leaked all customers credit cards and
billions of dollars were stolen".
A loss should always be something concrete
that someone cares about.


### Safety Control

```{=latex}
\begin{frame}
\frametitle{Safety Control}

Control\pause

Prevent loss

\end{frame}
```

A *safety control*
is a control whose purpose is to prevent or reduce loss.
For example,
a
*high availability load balancer*
will prevent loss in the form of a site outage
by routing to only healthy nodes.

A *safety control*
can also be a fire alarm.
While it does not prevent all loss from a fire,
it alerts people to leave,
thus preventing the loss of human life.


## CAST Pt 1.: Analyze

### Hazard

```{=latex}
\begin{frame}
\frametitle{Hazard}

Input to system\pause

led to problem

\end{frame}
```

When analyzing the system,
the
*hazard*
is an input to the system that led to the problem.
This is a little like the
"RCA",
but has a more concrete definition:

* It must come from outside the system
* There can be multiple hazards

The job of CAST is not to prevent the hazard.
*Knowing* the hazard is important regardless.

### Timeline

```{=latex}
\begin{frame}
\frametitle{Timeline}

What happened\pause

When

\end{frame}
```

The timeline should list everything relevant to the incident.
The hazard does not need to be the first thing!

Dates of changes to the system that are relevant,
for example,
often predate the hazard.
The timeline should not end before the incident was completely mitigated.

### Loss

```{=latex}
\begin{frame}
\frametitle{Loss}

Why it was bad?

\end{frame}
```

The next step is to explain the specific loss.
There was no incident if nobody was harmed.
Detail the specific loss incurred.

This can range anywhere from
"three people lost a work hour each restoring the system"
to...
well.

### Loss Analysis

```{=latex}
\begin{frame}
\frametitle{Loss Analysis}

Causal path\pause

Hazard\pause

Loss

\end{frame}
```

The loss analysis should be based on the timeline.
It should analyze the causal path that links the hazard
to the incident.
Each step in this should be documented in the timeline.

### Safety Control Failures

```{=latex}
\begin{frame}
\frametitle{Safety Control Failures}

For each safety control:\pause

Why it didn't prevent loss

\end{frame}
```

The hazard would not lead to the loss if the safety controls performed correctly.
For each relevant safety control,
indicate why it failed to prevent the loss.

## CAST Pt. 2: Improve

### Safety Control Problems

```{=latex}
\begin{frame}
\frametitle{Safety Control Problems}

Suggest fixes!

\end{frame}
```

For safety control that failed to prevent the loss,
explain what problem led to failing to do so.
Indicate how it could be fixed.

### Missing Safety Controls

```{=latex}
\begin{frame}
\frametitle{Missing Safety Controls}

Suggest implementation!

\end{frame}
```

Often,
the incident shows the lack of appropriate safety controls.
What additional safety controls were missing
that would have prevented the loss?

### Systemic Problems

```{=latex}
\begin{frame}
\frametitle{Systemic Problems}

Process\pause

Incorrect assumptions

\end{frame}
```

Finally,
this is a catch-all category.
What systemic problems in our processes led to this?

For example,
safety controls might be of poor quality
because they were rushed.
Alternatively,
maybe lack of testing of the controls is the issue.

### Improvement Plan

```{=latex}
\begin{frame}
\frametitle{Improvement plan}

Concrete\pause

Specific\pause

List

\end{frame}
```

An improvement plan should be written based on the issues above.
Each item in the improvement plan should be
*concrete*
and
*specific*.
Concrete means that it is something someone can do.
Specific means that it is clear when it has been done.

List all such items under the improvement plan.

## Summary

### Plan

```{=latex}
\begin{frame}
\frametitle{CAST: Plan}

Process ready

\end{frame}
```

An incident is going to happen.
Plan your
CAST-based process before,
so you know what do do
after.

### Apply

```{=latex}
\begin{frame}
\frametitle{CAST: Apply}

Incident?\pause

Assign\pause

Review\pause

Act

\end{frame}
```

After an incident,
assign someone to do the CAST investigation.
Once the report has been written,
have the team review it to see if anything
is missing or incorrect.
After the team signs off on it,
implement the improvement plan.

### Improve

```{=latex}
\begin{frame}
\frametitle{CAST: Improve}

CAST is a Safety Control

\end{frame}
```

CAST itself can be thought of a safety control.
Improvement plans should check previous incident retrospectives,
and see if anything needs to be done differently.

```{=latex}
\end{document}
```
