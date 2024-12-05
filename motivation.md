# Motivation and Rationale

Jupyter notebooks provide a means of distributing rich, editable documents to students that can also run provided or student generated code in a variety of languages as well as displaying code outputs.

First presented to students in an OU module in 2016, they are now being used in an increasing number of computing, maths and science based modules, with the potential to also support computational activities across the whole university curriculum.

However, making such environments available to students in secure enviroments presents certain challenges, given restrictions on software installation and access to unapproved websites.

Jupyter notebooks are accessed via a web-based user interface, with increasing support from other editor applications such as VS Code, RStudio/Posit.

Although the Jupyter user interface is typically accessed via a web browser, there are two important considerations:

- where is the user interface published from?
- where is the code edited the user interface actually executed?

Typically, both those considerations are handled by a Jupyter server running either on the student's own computer using self-installed software, or on a remotely hosted serve. Typically, Jupyter environments are made available to students:

- via virtual computing environments (VCEs) on OU hosted servers (for example, Computer Home/[OpenComputing Lab (OCL)](https://docs.ocl.open.ac.uk/container-launcher/user/));
- via locally run VCEs on students' own computers (e.g. using [Docker Desktop](https://docs.docker.com/desktop/));
- via local Anaconda/Python installations on students' own computers.

In each case, the Jupyter environment is accessed via a web browser.

However, there is an interesting new possibility of serving the Jupyter interface files from a simple, static (non-interactive) webserver (for example, as a Moodle VLE HTML activity) and then executing code *within the browser itself* using an application known as JupyterLite. The main advantages of JupyerLite are:

1) it runs in the browser, so no installation is required by students;

2) it runs in the browser, so the OU does not need to provide a hosted computing service;

3) it can be saved as a progressive web app with all required files cached within the browser, which means students should be able to work with it offline. 

In choosing which deployment route to use, Module Teams should consider:

- distribution and installation issues when installing software on students' own computers;
- resource costs (e.g. server hosting costs, network bandwidth costs, file storage costs, system administration and support costs);
- user experience issues, from managing files that have been modified or created by the students (*"can I keep copies of my edited files?"*), to computing resource requirements (*"can I physically run this on my own computer, and remove it when I'm done?"*), and to the availability of a network connection (*"can I work offline or do I need to be online?"*).

*See also: [Fragment — Some Rambling Thoughts on Computing Environments in Education](https://blog.ouseful.info/2019/03/20/fragment-some-rambling-thoughts-on-computing-environments-in-education/).*

## Using Jupyter Notebooks to Support OU Teaching and Learning

Typically, students work inside a Jupyter environment that sits "outside" the VLE context. In terms of learning material design, Jupyter notebooks have tended to be used to provide teaching and learning materials that are complementary to, but separate from, materials that are presented through the VLE.

```{admonition} Jupyter Notebooks in TM351
In *TM351 Data Management and Analysis*, student workload is split in eqwual measure across "conceptual" teaching materials presented via the VLE, and practical activities presented via Jupyter notebooks. THe Jupyter environment is delivered either via the OU hosted *Open Computing Lab*, or on students' own computers via a provided Docker container. Notebooks run Python code and can be used to access various database services bundled into the provided computing environment.
```

```{admonition} Jupyter Notebooks in TM112
In *TM112 Introduction to computing and information technology 2*, teaching and learning materials are primarily distributted through print media. The majority of practical activities use a simple Python distribution installed directly onto students' own computers, but a small collection of optional activities are provided using Jupyter notebooks. The notebooks are accessed either from the OU hosted OpenComputing Lab, locally on a student's own computer using a Docker container, or solely in the browser using a JupyterLite environment served from a public GiHub Pages hosted website.
```

```{admonition} Jupyter Notebooks in M348
An increasing number of modules in Maths and Science now make use of Jupyter notebooks to deliver computational activities. M348 uses notebooks run against an R kernel to perform a wide range of statistical analyses and generate a wide range statistical charts based on provided datasets. The ability to render maths formulae written in LaTeX in notebook markdown cells also means formulae can be rendered using a mathematical notation in explanatory, rather than computational, parts of the notebooks.
```

## Notebook Activity Design Considerations

The separation of notebooks from other course materials, whether print based or delivered via VLE content pages, forces a particular set of design constraints on notebook based activities.

There are three mains ways in which notebooks might be used to support teaching and learning in combination with VLE or print material presented conent:

1. Tightly coupled: for example, sparsely defined or student generated notebooks that contain bare coding examples. The student is led through the activity by instructions "at the side" presented through print materials or VLE content pages.

2. Weakly coupled: conceptual teaching content is provided via the VLE/print materials, with complementary practical activities presented in the notebooks. The notebooks contain enough instructional material to support the activity as a standalone document but the conceptual teaching is provided elsewhere;

3. Uncoupled / decoupled: all the content is provided in notebooks, with no other supporting materials.

## Embedding Practical Notebook Activities in the VLE

The ability to include rich HTML content in Jupyter notebooks allows us to present executable code based acitivites that are embedded within text based materials.

This rather begs the question as to why we might want to make use of the tightly or weakly coupled approaches described in the previous section — what benefits do we get from presenting materials via the VLE?

That question is perhaps best answered elsewhere, but it does raise another question: what benefits might we realise by being able to embed Jupyter notebook activities directly within VLE presented materials? And how might we go about embedding Jupyter activities directly within the VLE?
