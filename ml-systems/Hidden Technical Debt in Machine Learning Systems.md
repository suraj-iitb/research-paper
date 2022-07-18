![image](https://user-images.githubusercontent.com/20740522/179568153-61cff127-eda0-444b-a5b0-ce032947863d.png)

## Anti Patterns

1. Glue Code
    - Massive amount of supporting code (>=95%) is written to get data into and out of general-purpose packages written by ML researchers
    - Glue code is costly in the long term because
        - Freezes a system to the peculiarities of a specific package
        - Testing alternatives may become prohibitively expensive
        - Inhibit improvements, because it makes it harder to take advantage of domain-specific properties or to tweak the objective function to achieve a domain-specific goal
    - Wrap black-box packages into common API’s to combat glue code
    - Root cause in overly separated research and engineering roles
2. Pipeline Jungles
    - Appears in data preparation
    - As new signals are identified and new information sources added incrementally thus resulting system for preparing data in an ML-friendly format may
become a jungle of scrapes, joins, and sampling steps, often with intermediate files output
    - Managing, testing (end-to-end integration tests), detecting errors and recovering from failures are all difficult and costly
    - Can be avoided by thinking holistically about data collection and feature extraction
    - Root cause in overly separated research and engineering roles
3. Dead Experimental Codepaths
    - A common consequence of glue code or pipeline jungles is that it becomes increasingly attractive in the short term to perform experiments with alternative methods by implementing experimental codepaths as conditional branches within the main production code.
    - For any individual change, the cost of experimenting in this manner is low. However, over time, these accumulated codepaths can create a growing debt due to the increasing difficulties of maintaining backward compatibility and an exponential increase in cyclomatic complexity.
4. Abstraction Debt
    - What is the right interface to describe a stream of data, or a model, or a prediction?
    - For distributed learning in particular, there remains a lack of widely accepted abstractions
        - Widespread use of Map-Reduce in machine learning was driven by the void of strong distributed learning abstractions
        - Map-Reduce is a poor abstraction for iterative ML algorithms
    - The parameter-server abstraction seems much more robust than Map-Reduce abstraction
5. Common Smells
    - Indicates an underlying problem in system
        - Plain-Old-Data Type Smell: Encoded with plain data types like raw floats and integers
        - Multiple-Language Smell: Increases the cost of effective testing and the difficulty of transferring ownership to other individuals
        - Prototype Smell: Relying on a prototyping environment may be an indicator that the full-scale system is brittle, difficult to change
    
## Configuration Debt

1. Debt in the configuration of machine learning systems
2. Configurations like
    - Which features are used
    - How data is selected
    - Algorithm-specific configurations
    - Potential pre-processing & post-processing
    - Verification methods

### Why Configuration Debt?

1. Not treating configurations as important (even though no. of lines of configuration is more than no. of lines of code)
2. Configuration example:
    - Feature A was incorrectly logged from 9/14 to 9/17
    - Feature B is not available on data before 10/7
    - Code used to compute feature C has to change for data before and after 11/1 because of changes to the logging format
    - Feature D is not availble in production, so a substitute features D<sup>'</sup> & D<sup>''</sup> will be used when querying the model in production
    - If feature Z is used, then jobs for training must be given extra memory due to lookup tables
    - Feature Q precludes the use of feature R because of latency constraints
3. Mistake in configuration can be costly
    - Loss of time
    - Waste of computing resources
    - Production issues

### Principles of Good Configuration Systems

1. Easy to specify a configuration as a small change from a previous configuration
2. Easy to see, visually, the difference in configuration between two models
3. Easy to automatically assert and verify basic facts about the configuration: number of features used, transitive closure of data dependencies etc
4. Hard to make manual errors, omissions, or oversights
5. Detect unused or redundant settings
6. Configurations should undergo a full code review and be checked into a repository
