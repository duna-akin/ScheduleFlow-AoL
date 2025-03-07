# Tour Guide Scheduling Automation - Project Proposal

## Lafayette College Office of Admissions | Ambassadors of Lafayette

### Summary

Proposed automation system to streamline tour guide scheduling and communication processes for the Lafayette College Admissions Ambassadors of Lafayette program.

### Current Challenges

- Manual scheduling process is time-intensive
- Risk of human error in assignments
- Complex coordination of multiple tour guides
- Time-consuming manual Slack notifications
- Difficulty tracking guide availability
- Difficulty assuring even distribution of tour guide workload

### Proposed Solution

Develop an automated system that will:

1. Process tour guide availability from already utilized Excel spreadsheets
2. Match available guides with family/visitor demand
3. Generate optimized daily/weekly schedules
4. Automatically send formatted Slack notifications via a Slack-Bot integration
5. Handle special assignments (co-tours, evaluations)

### Technical Implementation

- Python-based scheduling engine
- Excel/CSV data processing
- Slack API integration with a Slack-Bot
- Possible Slate integration (pending approval)

### Benefits

- Potential 80% reduction in scheduling time
- Improved accuracy in assignments
- Enhanced communication efficiency
- Better utilization of guide availability
- Reduced administrative overhead

### Resource Requirements

- Development time: 3-5 weeks
- Testing period: 2 weeks
- Technical resources:
  - Server/computer for running scripts
  - Slack workspace access
  - Python development environment
  - Excel/CSV processing capabilities

### Success Metrics

- Reduction in scheduling time
- Decrease in scheduling conflicts
- System reliability measurements

### Risk Management

- Data backup procedures
- Manual override capabilities
- Fallback scheduling process
- Regular system monitoring

### Next Steps

1. (DONE) Approval
2. Technical requirements finalization
3. Development kickoff
4. Initial testing phase
5. Pilot program

### Ideas from Meeting w/ Brady

- **Tour Guide Classification System**: Implement a tiered classification (experienced/regular/new) based on amount of tours given to optimize assignments for evaluations and co-tours.
- **Visitor Interface**: Develop a streamlined user interface for quickly entering and updating visitor counts.
- **Comprehensive Coverage Management**:
  - Track individual guide metrics including completed tours, missed assignments, and excused absences
  - Integrate with office worker check-in system to verify tour completion and automatically update guide statistics
  - Design an intuitive dashboard for office workers to log guide status (checked in, completed tour, absent)
  - Implement backup guide assignment protocol for single-family tours to prevent no-show scenarios
- **Automated Communication Tools**:
  - Create system for automatically sending tour details to assigned guides (visitor counts, special accommodations, check-in location, who will break apart families...)
    - *Assigned Tour Guides: [TG-tag] [TG-tag] ...*
    - *Tour Time: [hour] [date]*
    - *Check-in will be from [location]*
    - *There are [visitor-amount] families registered.*
    - *[TG-tag] will break up the families!*
  - Develop streamlined excuse submission and processing workflow, with automatic removal from assignment pool and attendance tracking.
  
  ### Ideas from Meeting w/ Camille for Specialty Tours

  - Create a counter variable for the amount of requirements a tour guide must complete and actively update it with each specialty tour they successfully complete.
    - Make it fair by not making a single tour guide go on more specialty tours than another tour guide for example, specialty tours are hard.
  - Try to have steps, at first try to assign one tour guide per 5 families, if that isn't enough do 10, then 20.
  - Implement a visual progress tracker showing each guide's completion status toward specialty tour requirements
  - Include a breakdown of different specialty tour types completed (e.g., Saturday tours, group tours, open house tours)
  - Add a forecasting tool to identify which guides need more specialty tour opportunities to meet requirements
  - Balanced Assignment Algorithm:
    - Create a weighted assignment system that considers:
      - Current specialty tour count per guide
      - Time since last specialty tour assignment
      - Guide availability and schedule load
  - Tour detail messages communicated in Slack will be same as regular tour messages.
  - Add specialized feedback forms for specialty tour participants
    - Incorporate feedback scores into guide specialty tour qualification metrics
    - Create improvement path for guides with lower specialty tour performance ratings
