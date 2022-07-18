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
