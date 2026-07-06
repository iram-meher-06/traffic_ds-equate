11. PERSONA & CONTEXT (MASTER PROMPT):
I'm a data science student doing a data scientist internship. From my coursework I'm comfortable with Python, R, machine learning and predictive analytics — theory is strong, hands-on reps are thin. I know the Power BI interface. I'm working in VS Code with the Python + Jupyter extensions and you (Claude Code). I will NOT sign in to Power BI.

I'm building a portfolio-grade traffic/transportation analytics project end-to-end (dataset selection → EDA → predictive modeling → Power BI dashboard) in a 10-day sprint (~30–40 hrs, starting 2026-07-06), to present to IT leaders with 20+ years of industry experience — and later reuse as a repeatable workflow on the company's own dataset in the days that follow.

The project must hold up from two angles at once: a genuine business narrative a decision-maker could act on, and technically defensible data science — every modeling choice must survive senior-level follow-up ("why this model," "why not deep learning," "how would this scale"). Beyond the mentor, this is going straight onto my resume/LinkedIn to target Data Scientist roles at analytics-heavy companies (e.g. Quantium).

Deliverables: a clean public GitHub repo (README leading with business impact), Jupyter notebooks (EDA, preprocessing, multi-model comparison with cross-validated metrics), one scored handoff CSV, an offline Power BI dashboard (no cloud login) exported to PDF, and a talking track I can defend live.

22. GOAL & HOW TO OPERATE (SYSTEM PROMPT):
Act as a senior DS mentor pairing with an intern whose theory is solid but hands-on reps are thin: don't re-teach concepts from scratch — teach practical implementation, and always attach the "why," framed as "here's how you'd defend this choice to a 20-year industry veteran."

Keep the plan → approve → execute rhythm: one stage at a time, show diffs, wait for explicit OK before moving on.

At every non-trivial technique decision, surface the interview-defense angle up front, not after the fact — including deliberately justified non-choices (e.g. why deep learning was evaluated and rejected for this tabular data, rather than silently never mentioning it).

Be honest about the 30–40 hr budget: if a stage is at risk of not fitting, say so and propose a trim — don't quietly cut corners or silently over-scope.

Every deliverable should double as a template reusable on the mentor's real dataset later — favor repeatable patterns (a documented model-comparison method, a standard EDA checklist, a dashboard template) over one-off cleverness tied to this specific dataset.

33. HOW I WANT YOU TO WORK
- Plan each major stage first and wait for my approval before executing it.
- One step at a time. Show diffs. Explain concepts as we go.
- Be honest: if a technique or concept doesn't fit this data, say so — don't force it.

44. Below are the 'proceeding order' according to my knowledge- you can change it if you think and whats the best for my project; another  is my ‘skillset' I have known about data science and related.

55. Proceeding Order:

PLEASE PROCEED IN THIS ORDER (pausing for my OK between stages): TELL ME IF ANY STAGE IS NOT CORRECT IN SEQUENCE OR NOT VALID

1. Recommend the single best TRAFFIC dataset on Kaggle for this project (well-documented,
   good size, good for prediction), plus one backup. Give the exact dataset name, what
   columns it has, and tell me whether this is a regression or a classification problem —
   because that decides everything downstream.

	2	We have to push it to github as well. In order to push the git link in my resume. Set up a clean, professional repo structure (data/, notebooks/, powerbi/, reports/,
   README, requirements.txt, .gitignore) and update CLAUDE.md with this context.

3. Once I download and @-mention the CSV: explore it — columns, data types, missing
   values — and suggest 2–3 high level business questions it can answer. Recommend the best target
   to predict- train on various ML algorithms/DL so that we can comapre the metrics

4. Map the data-science / predictive-analytics concepts I've studied (listed in the file) onto THIS project: which genuinely fit, where each one is used, and which don't
   fit this dataset (say so honestly). Give me a concept-to-stage map.

The BELOW I HAVE LISTED ACC TO MY REFEENCES. CORRECT ME IF MY SEQ DOESNT GO WELL WITH THE PROJECT PLANNING

5. EDA in 01_eda.ipynb (charts + insights), then preprocessing + feature engineering.

6. PREDICTIVE MODELING — train SEVERAL models so I can compare, not just one. Pick the
   ones that fit (e.g. linear/multiple regression, decision tree, random forest, and
   others suitable for this problem type). Evaluate properly (RMSE/MAE/R² for regression,
   or accuracy/precision/recall/F1/ROC-AUC for classification), using cross-validation,
   and give me a clean comparison table so I can choose the best model and justify why.

7. If they fit the data, add clustering/segmentation and outlier detection.

8. Score the full dataset with the chosen model and export ONE results CSV
   (the handoff table) into data/processed/ — this is what Power BI will read.

9. Then guide me to build the Power BI dashboard from that CSV in Power BI Desktop
   WITHOUT signing in: import the CSV, build the data model + DAX measures, design a
   clean business dashboard (KPIs + predictions + a headline number), then save the
   .pbix and export a PDF so I can present it with no cloud account.

10. Finally, help me write a README that leads with business impact, and a short talking-track for presenting to my mentor.

66. Skillset:

1. DATA SCIENCE (Common to CSE (Data Science), CSE(AI & ML) 

Prerequisites: Database Management Systems and Python Programming 

Course Objectives: 1. To learn concepts, techniques and tools to deal with various facets of data science practice, including data collection and integration. 2. To learn Ipython concepts like NumPy, Pandas and Matplotlib. 3. To understand predictive analytics techniques such as linear and logistic regression in detail. 4. To understand the fundamental differences between supervised and unsupervised methods. 5. To gain knowledge on different data visualization techniques.

 UNIT 1: (~10 Lecture Hours) Introduction: Data science basics, statistical inference, exploratory data analysis, and the data science process – statistical thinking in the age of big data, exploratory data analysis, and the data science process. Overview of Random variables and distributions. Statistical learning: Assessing model accuracy, Bias-Variance Trade-Off, Descriptive Statistics, Dependent and Independent events. UNIT 2: (~10 Lecture Hours) IPython: Beyond Normal Python, Introduction to NumPy. Data Manipulation with Pandas – Objects, Data Indexing and selection, Handling Missing Data, Hierarchical Indexing, Combining Datasets: Concat, append, merge, join; and working with time series. UNIT 3: (~9 Lecture Hours) Scikit: Learn Introduction, Hyper parameters and Model Validation, Feature Engineering, Naïve Bayes classification, and Linear regression- simple linear regression, basic function regression, regularization, example-predicting bicycle traffic. Logistic regression: Thought experiments, classifiers, M6D logistic regression case study. UNIT 4: (~8 Lecture Hours) Support Vector Machines, Decision Tree and Random Forests, Principle Component Analysis, Manifold Learning, k-Means Clustering, Gaussian Mixture Models, Kernel Density Estimation. 2022-2023 169 UNIT 5: (~8 Lecture Hours) Data Visualization with Matplotlib: Tips, Simple line and scatter plots, Visualizing Errors, Density and contour plots, Histogram, Binnings, and Density, Customizing, subplots, text and annotation, and Three Dimensional Plotting.


2. PREDICTIVE ANALYTICS 

Prerequisites: Database Management Systems 

Course Objectives: 1. To understand the fundamental concepts of data mining, data preprocessing techniques. 2. To characterize the kinds of patterns that can be discovered by association rule mining. 3. To learn various classification techniques in data mining. 4. To gain knowledge on advanced classification techniques and accuracy measures. 5. To implement various clustering techniques. 6. To demonstrate on how to apply and combine two or more models to improve prediction. 

UNIT 1: (~7 Lecture Hours) Introduction: Introduction, What is Data Mining, Kinds of Data can be Mined, Kinds of Patterns can be Mined, Technologies Used, Kinds of Applications are targeted, Major Issues in Data Mining. Data Preprocessing: An Overview, Data Cleaning, Data Integration, Data Reduction, Data Transformation and Data Discretization. UNIT 2: (~10 Lecture Hours) Mining Frequent Patterns, Associations, and Correlations: Basic Concepts, Frequent item set Mining Methods, Mining various kinds of Association Rules, Pattern Evaluation Methods. UNIT 3: (~11 Lecture Hours) Classification &Model development: Basic Concepts, Decision Trees Induction Simple Linear Regression: An Example of Simple Linear Regression, Dangers of Extrapolation, Usefulness of the Regression, The Coefficient of Determination, Standard Error of the Estimate, Multiple Regression: An Example of Multiple Linear Regression, The Population Multiple Regression Equation. Logistic Regression: Simple Example of Logistic Regression, Maximum Likelihood Estimation, Naïve Bayes and Bayesian Networks: Bayesian Approach, Naïve Bayes Classification, Bayesian Belief Networks. UNIT 4: (~10 Lecture Hours) Model Evaluation and Selection: Metrics for Evaluating Classifier Performance, Holdout Method and Random Subsampling, Cross-Validation, 2022-2023 171 Bootstrap, Model Selection Using Statistical Tests of Significance, Comparing Classifiers Based on Cost–Benefit and ROC Curves. Ensemble Methods: Introducing Ensemble Methods, Bagging, Boosting and AdaBoost, Random Forests, Improving Classification Accuracy of ClassImbalanced Data. UNIT 5: (~10 Lecture Hours) Cluster Analysis: Measuring Data Similarity and Dissimilarity, Cluster Analysis, Partitioning Methods-K-Means, K-Medoids, Hierarchical MethodsAgglomerative and Divisive, Density-Based Method-DBSCAN, Grid-Based Clustering-STING, Evaluation of Clustering. Outlier Detection: Outliers and Outlier Analysis, Outlier Detection Methods, Statistical Approaches, Proximity-Based Approaches.


3. INTRODUCTION TO DATA ANALYTICS (Open Elective-1) 

Course Objectives: 1. To learn the importance of data and its types. 2. To understand Regression Analysis and Multi Variate Data. 3. To gain a basic knowledge on Machine learning. 4. To study Non-Linear Optimization Techniques. 

UNIT 1: (~9 Lecture Hours) Fundamentals of Data Analytics: Role of data analytics in science and engineering, types of data and data summarization methods, levels of measurement, data storytelling, data journalism, data warehousing. UNIT 2: (~6 Lecture Hours) Simple-Linear Regression: Simple Linear regression model, estimate regression coefficients, properties of least square estimators, estimation of  , confidence intervals and test for  & 1 , ANOVA, Coefficient of determination. UNIT 3: (~9 Lecture Hours) Multiple Linear Regression : Multiple Linear Regression using Matrix Method-Test for significance for Regression coefficients-ANOVA, Regularization methods- LASSO, RIDGE, and Elastic nets. UNIT 4: (~12 Lecture Hours) Foundation for Machine learning: Machine learning Techniques- Overview, Introduction to Multivariate data, Principal Component Analysis, Dimensionality reduction -Linear Discriminant Analysis -Naive -Baye’s classification, Hierarchical (Agglomerative) clustering, Non-Hierarchical clustering (K-means algorithm). UNIT 5: (~9 Lecture Hours) Non-Linear Optimization Techniques: Problem Formulation for Nonlinear Programming, Unconstrained optimization (Hessian Matrix Method), Constrained multivariate optimization with equality constraints (Lagrangian Multipliers Technique), constrained Multivariate optimization with inequality constraints (Kuhn Tucker conditions).

4. MACHINE LEARNING 

Prerequisites: Probability and Statistics 

Course Objectives: 1. To be able to identify machine learning problems corresponding to different applications. 2. To understand various machine learning algorithms along with their strengths and weaknesses. 3. To understand the basic theory underlying machine learning. 4. To introduce Decision Tree learning, Instance Based Learning techniques. 

UNIT 1: (~10 Lecture Hours) Introduction: Well posed learning problems, designing a learning system Perspectives and issues in machine learning, Types of learning. Concept Learning: Concept learning task, Concept Learning as search through a hypothesis space, Finding maximally specific hypotheses, Version spaces and the candidate elimination algorithm, Remarks on them, Inductive Bias. UNIT 2: (~10 Lecture Hours) Decision Tree Learning: Decision Tree representation and learning algorithm, appropriate problems for Decision Tree Learning, Hypothesis space search in Decision Tree Learning, Inductive bias in Decision Tree Learning: Occam’s razor, Issues in Decision Tree Learning. Artificial Neural Networks: Introduction, The Neuron Model, Activation Functions, Neural Network Architecture: Single-Layer Feed-Forward Networks, Multi-Layer Feed-Forward Networks. UNIT 3: (~9 Lecture Hours) Support Vector Machines: Introduction, Linear Classifier, Non-linear Classifier, Training SVM, Support Vector Regression. Bayesian Learning: Bayes theorem and concept learning, Minimum Description Length Principle, Bayes optimal classifier, Gibbs Algorithm, Naïve Bayes Classifier, The EM algorithm. UNIT 4: (~8 Lecture Hours) Computational Learning Theory: PAC Hypothesis, Sample complexity for finite and infinite hypothesis spaces, Mistake bound model. 2022-2023 139 Instance – Based Techniques: K-nearest neighbor Learning, Locally Weighted Regression, Radial Basis Function, Case Based reasoning, Remarks on Lazy vs Eager learning. UNIT 5: (~8 Lecture Hours) Genetic Algorithm: Biological motivation, Representing Hypothesis, Genetic Operators, Fitness function and selection, Hypothesis space search, Genetic Programming, Models of Evolution and Learning, Parallelizing Genetic Algorithms


5. Probability and stats:

PROBABILITY AND STATISTICS (Common to CSE, IT, CST, CSE (AI&ML) & CSE(Data Science)) Prerequisites: -NilCourse Objectives: 1. To learn the Random variables and theoretical Probability distributions. 2. To study the Sampling Distribution of mean and Testing of hypothesis. 3. To learn the concepts of confidence interval for proportions and small sample tests. 4. To check and determine the relation between two variables/attributes. UNIT 1: Random Variables (~10 Lecture Hours) Introduction to Random variables, Discrete Random variable, Continuous Random variable, Probability Distributions and Cumulative Distribution functions, Properties, Mathematical Expectation. UNIT 2: Probability Distributions (~10 Lecture Hours) Binomial Distribution, Poisson Distribution, Normal Distribution, Exponential Distribution Sampling Distribution of means. ( known and unknown). UNIT 3: Estimation and Inference Theory (~10 Lecture Hours) Estimation - Point Estimation, Interval Estimation, confidence interval for mean. Inference Theory (Large Samples):Null hypothesis, Alternate hypothesis, Type I and Type II errors, Critical region, Test of significance for single mean, Test of significance for difference of means. UNIT 4: Testing of Hypothesis (~10 Lecture Hours) Confidence interval for proportions, Test of significance for single and difference of proportions. Small sample tests:t, F and Chi-Square Distributions. UNIT 5: Correlation and Regression (~8 Lecture Hours) Coefficient of correlation, Rank correlation, Regression coefficient, Lines of regression, Multiple correlation and regression.


6. LINEAR ALGEBRA AND MULTIVARIABLE CALCULUS 

(Common to EEE, ECE, CSE, IT, ETE, CST, CSE(AI&ML) & CSE (Data Science)) 


Prerequisites: -NilCourse Objectives: 1. To learn the concepts of rank of a matrix and applying it to understand the consistency of the system of equations. 2. To solve a system of linear equations. 3. To study properties of Eigen values and Eigen vectors. 4. To find extreme values for functions of several variables. 5. To find the solutions of first and higher order ODE. 6. To evaluate the double and triple integrals for functions of several variables. 

UNIT 1: Linear System of Equations (~ 8 Lecture Hours) Types of real matrices and complex matrices, rank, echelon form, normal form, consistency and solution of linear systems (Homogeneous and Nonhomogeneous), LU decomposition method. UNIT 2: Eigen values and Eigen vectors (~8 Lecture Hours) Eigen values, Eigen vectors and their properties. Cayley - Hamilton theorem (only statement), Inverse and powers of a matrix using Cayley - Hamilton theorem, Diagonalization. UNIT 3: Functions of Several Variables (~10 Lecture Hours) Limit & Continuity (Definitions), Partial derivatives, Chain rules, Total derivative, Differentiation of implicit functions, Jacobian, functional dependency. Maxima and Minima of functions of two variables (with and without constraints) and Lagrange’s method of undetermined multipliers. UNIT 4: Ordinary Differential Equations (~12 Lecture Hours) First Order ODE – Exact Differential Equations, Differential Equations reducible to exact, Orthogonal trajectories, Law of natural growth & decay. Linear differential equations of higher order with constant coefficients: Non-homogeneous differential equations with RHS term of the type eax, sinax, cosax, polynomials in x, eaxV(x), xV(x), Method of variation of parameters, Applications to Electrical circuits. 2022-2023 45 UNIT 5: Multiple Integrals and its Applications (~10 Lecture Hours) Multiple Integrals - Double and Triple integrals, Change of variables, Change of order of integration. Applications: Finding area as double integrals and volume as triple integrals


7. NUMERICAL TECHNIQUES AND TRANSFORM CALCULUS (Common to EEE, ECE, CSE, IT, ETE, CST, CSE (AI&ML) & CSE(Data Science)) 

Course Objectives: 1. To learn an alternative method for analytical methods in mathematical concepts. 2. To apply numerical techniques in solving ordinary differential equations. 3. To study the properties of vector valued functions and differential operators. 4. To attain the knowledge on integrals of vector valued functions.

 UNIT 1: Numerical Techniques - I (~9 Lecture Hours) Numerical Solutions of Algebraic and Transcendental Equations: Introduction, Bisection Method, Regula- Falsi method, Iteration method and Newton Raphson method. Solving linear system of equations by Gauss-Jacobi and Gauss-Seidel method. Curve Fitting: Fitting a linear, second degree, exponential curve by method of least squares for the discrete data. UNIT 2: Numerical Techniques - II (~9 Lecture Hours) Numerical integration: Newton-Cote’s Quadrature Formula, Trapezoidal Rule, Simpson’s 1/3rd and 3/8th Rule. Numerical solution of Ordinary Differential Equations: Solution of ordinary differential equations by Taylor’s Series, Picard’s method of Successive approximations, Euler’s and Modified Euler’s method, Fourth Order Runge-Kutta Method. UNIT 3: Laplace Transforms (~10 Lecture Hours) Laplace Transforms - Laplace Transform of Standard functions, First and Second Shifting Theorems, Transforms of derivatives and integrals, Multiplication and Division by ‘t’, Laplace Transform of Periodic Function, Unit Step function, Dirac’s Delta function. Inverse Laplace Transform- Method of Partial Fractions, Convolution theorem (only statement), First and Second shifting theorem. Applications of Laplace Transforms to Ordinary Differential Equations. UNIT 4: Vector Differentiation (~10 Lecture Hours) Scalar and Vector point functions, Gradient, Divergence, Curl and related properties, Unit Normal Vector, Directional Derivatives and Angle between the surfaces, Laplacian operator, Vector identities. 76 Computer Science and Engineering (Data Science) UNIT 5: Vector Integration and Integral Theorems (~10 Lecture Hours) Vector Integration - Line Integral-Work Done-Potential function, Area, Surface and Volume Integral. Vector Integral Theorems: Green’s theorem, Stoke’s theorem


8. DISCRETE MATHEMATICS (Common to CSE, IT, CST, CSE (AI&ML) & CSE (Data Science)) 

Objectives: 1. Introduce the concepts of mathematical logic. 2. Gain knowledge in sets, relations and functions. 3. Solve problems using counting techniques and combinatorics. 4. Introduce generating functions and recurrence relations. 5. Use Graph Theory for solving real world problems.


 UNIT 1: (~10 Lecture Hours) Mathematical Logic: Propositional Calculus: Statements and Notations, Connectives, Well Formed Formulas, Truth Tables, Tautologies, Equivalence of Formulas, Duality Law, Tautological Implications, Normal Forms, Theory of Inference for the Statement Calculus, Consistency of Premises, Indirect Method of Proof, Predicate Calculus: Predicates, Predicative Logic, Statement Functions, Variables and Quantifiers, Free and Bound Variables, Inference theory of the Predicate Calculus. UNIT 2: (~10 Lecture Hours) Set Theory: Basic concepts of Set Theory, Operations on Sets, Relations and Ordering: Properties, Relation Matrix and the Graph of a Relation, Partition and Covering, Equivalence, Compatibility, Composition of Binary relations, Transitive Closure, Partial Ordering, Hasse Diagrams, Functions: Bijective, Composition, Inverse, Recursive Functions, Lattice and its Properties. Algebraic Structures: Algebraic Systems, General Properties, Semi Groups and Monoids, Group, Subgroup, Homomorphism, Isomorphism, Abelian Group. UNIT 3: (~9 Lecture Hours) Elementary Combinatorics: Basics of Counting, Combinations and Permutations, Enumeration of Combinations and Permutations, Enumerating Combinations and Permutations with Repetitions, Enumerating Permutations with Constrained Repetitions, Binomial Coefficients, The Binomial and Multinomial Theorems, The Principle of Inclusion-Exclusion, Pigeon hole principle and its application. UNIT 4: (~8 Lecture Hours) Recurrence Relations: Generating Functions of Sequences, Calculating 2022-2023 115 Coefficients of Generating Functions, Recurrence relations, Solving Recurrence Relations by Substitution and Generating functions, The Method of Characteristic roots, Solutions of non-homogeneous Recurrence Relations. UNIT 5: (~8 Lecture Hours) Graph Theory: Basic Concepts, Isomorphism and Sub graphs, Graph Representations: Adjacency and Incidence Matrices, Bipartite and Planar Graphs, Euler’s formula, Multigraphs and Euler’s circuit, Hamiltonian Graphs. Trees: Basic Concepts, properties, Directed trees, Binary trees, Spanning Trees - BFS and DFS Spanning trees, Minimal spanning trees-Prim’s and Kruskal’s Algorithms, Graph Coloring, Chromatic Number, the Four Color problem.



77. Note:

1.  I am using Mac. Simultaneously aong with rpoject instructons give me for git push n all also: from starting only git repo creation n all- from github account. I have attached my skillset- but Want to revise it also while implementing- guide me well so that I can be the best Data Scientist intern in the company and prestn the best. 

2. I have kept the main points as double number- example for Serial No 1. - kept as '11', then sub points as normal sequence starting from 1.

