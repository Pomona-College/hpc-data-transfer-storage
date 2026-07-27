# Learner Profiles

This workshop is designed for researchers at Pomona College who use the Sagehen HPC cluster. Below are examples of learners who might attend and what they hope to accomplish.

## Profile 1: Alice - Graduate Student, New to HPC

**Background:**
- First-year graduate student in biology
- New to HPC and high-performance computing
- Has small laptop with limited storage
- Works on genomic analysis project with 200 GB of raw data

**Current Challenges:**
- Doesn't know where to store large datasets
- Has transferred files manually one-by-one, very slow
- Lost intermediate results when job finished
- Quota fills up unexpectedly
- Unsure how to organize project files

**Goals for Workshop:**
- Learn to organize project files efficiently
- Understand different storage options
- Find faster way to transfer large files
- Learn to use temporary storage for jobs
- Implement backup strategy

**Why This Matters:**
- Alice's research productivity depends on efficiently managing 200+ GB of data
- Learning proper storage use saves her hours per week in transfers
- Understanding temporary storage will reduce her job runtimes by 30-40%
- Proper organization prevents data loss and makes collaboration easier

## Profile 2: Bob - Faculty Member, Heavy Computation User

**Background:**
- Physics faculty with active research group (5 students)
- Has been using Sagehen for 2 years
- Runs many parallel jobs generating output daily
- Lab shares 1TB quota among all members
- Uses command-line for most work

**Current Challenges:**
- Lab frequently hits quota limit, blocking jobs
- Jobs run slowly because they read/write to home directory
- Hard to share simulation results with students
- Doesn't know how to clean up old data efficiently
- Wants to automate data transfer processes

**Goals for Workshop:**
- Learn advanced rsync techniques for bulk transfers
- Optimize job performance using temporary storage
- Implement automated backup strategy
- Plan how to archive completed projects
- Set up scripts for regular data management

**Why This Matters:**
- Bob's research group is blocked by quota issues
- Faster job I/O will accelerate research timeline
- Automated transfers save time for all group members
- Proper archival reclaims quota for active research

## Profile 3: Carol - Undergraduate Research Student

**Background:**
- Junior undergrad in computer science
- Working on machine learning project
- Some command-line experience, prefers GUI when possible
- Has laptop with 256 GB SSD (reasonably fast)
- First time using shared storage system

**Current Challenges:**
- Uncomfortable with command-line tools
- Doesn't understand where to save files
- Training data is 50 GB (laptop not big enough)
- Wants easy way to transfer between laptop and Sagehen
- Unsure how permissions work for shared files

**Goals for Workshop:**
- Learn to use OnDemand web interface
- Use FileZilla for interactive file transfers
- Understand permission and sharing
- Keep project organized for thesis
- Learn best practices from the start

**Why This Matters:**
- Carol's learning curve is steeper without HPC background
- GUI-based tools make learning more accessible
- Early good habits prevent problems later
- Building confidence in young researchers strengthens research community

## Profile 4: David - Post-Doc, Data-Intensive Research

**Background:**
- Postdoctoral researcher in chemistry
- Works with crystallographic data (100s of GB per project)
- Collaborates with team at another institution
- Needs to move data between institutions regularly
- Experienced with HPC, wants to optimize workflows

**Current Challenges:**
- Large files transfer slowly between institutions
- Temporary storage fills up during jobs
- Needs to share large datasets with collaborators
- Complex pipeline produces intermediate files
- Wants to implement checkpointing for long jobs

**Goals for Workshop:**
- Master rsync for efficient transfers
- Use temporary storage effectively in complex pipelines
- Implement checkpoint/restart patterns
- Learn best practices for sharing large datasets
- Optimize storage for multi-week computational runs

**Why This Matters:**
- David's research depends on efficient data movement
- Learning temporary storage techniques will save 50+ hours per project
- Checkpointing patterns enable better resource utilization
- Optimization techniques are transferable to future positions

## Profile 5: Evelyn - Course Instructor

**Background:**
- Teaches computational biology course
- Each semester has 20+ students using Sagehen
- Needs to manage class data and student access
- Wants to provide clean examples for assignments
- Concerned about quota management for class

**Current Challenges:**
- Students don't understand where to store files
- Reference data for assignments clutters storage
- Hard to collect student results for grading
- Each student's poor practices affect group quota
- Managing permissions for shared class data

**Goals for Workshop:**
- Learn to organize class data effectively
- Understand how to set up shared space for assignments
- Learn to enforce good practices via examples
- Understand quota implications of 20+ students
- Find efficient ways to collect and archive student work

**Why This Matters:**
- Evelyn's teaching effectiveness depends on good infrastructure
- Well-organized class storage enables better learning
- Teaching best practices early prevents problems later
- Managing class quota efficiently ensures all students can work

## Common Learning Needs Across Profiles

All learners want to:

1. **Understand the system** - Where does data go? What are the tradeoffs?
2. **Save time** - Transfer files faster, spend less time managing storage
3. **Avoid disasters** - Not lose work, prevent quota-related job failures
4. **Collaborate** - Share data with lab members and external collaborators
5. **Scale up** - Handle increasingly large datasets as research grows

## Workshop Design Decisions Based on Learner Profiles

The workshop is structured to serve all these profiles:

- **Early episodes** (Storage, Quotas) cover fundamentals for everyone
- **Multiple methods** (OnDemand, FileZilla, rsync) for different comfort levels
- **Real examples** drawn from actual research scenarios
- **Practical challenges** use workflows participants actually do
- **Both GUI and CLI** tools to serve different learning styles
- **Advanced content** (temporary storage, automation) for experienced users
- **Best practices** section teaches institutional knowledge

## Accommodation Considerations

Learners may have different needs:

- **Visual learners**: GUI demos (OnDemand, FileZilla) emphasized
- **Command-line comfort**: rsync and scripting episodes detailed
- **Different operating systems**: Instructions for Windows, macOS, Linux
- **Varying technical backgrounds**: Fundamentals explained before advanced
- **Accessibility**: Workshop materials available in multiple formats

## Success Metrics by Profile

**Alice (Student):**
- Can transfer 500 MB file in <2 minutes
- Organized her project structure before workshop ends
- Understands where her data actually lives

**Bob (Faculty):**
- Has working rsync backup script
- Can estimate impact of temporary storage on job runtime
- Lab quota management plan established

**Carol (Undergrad):**
- Successfully uploaded project data via FileZilla
- Understands permissions for shared lab files
- Set up project directory structure

**David (Postdoc):**
- Knows how to optimize pipeline using temporary storage
- Can write rsync commands with multiple exclusions
- Understands data lifecycle for long projects

**Evelyn (Instructor):**
- Has organization plan for class data
- Can teach students proper storage practices
- Knows how to manage quota with multiple students
