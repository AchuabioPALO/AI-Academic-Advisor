---
mode: agent
---

# AI Academic Advisor - POC Implementation Prompt

### instructions for AI
- keep going until the job is completely solved before ending your turn
- use your tools and dont guess
- plan throroughly before every tool call
- if you are not sure about file content or codebase structure , open them- do not hallucinate
- you MUST plan extensively before each function call, and reflect extensively on the output of the previous function call

## Project Overview
Build a **Proof of Concept (POC)** for an AI-powered Academic Advisor that demonstrates core functionality in 3-5 days. This POC will show stakeholders the value proposition without building a full production system.

## POC Goals
1. **Demonstrate Concept Viability**: Show that AI can effectively guide students academically
2. **Get Stakeholder Buy-in**: Prove the system solves real student problems
3. **Validate Technical Approach**: Confirm the architecture can scale to production
4. **Secure Funding/Approval**: Use demo to get resources for full implementation

## Core Features to Implement (SIMPLIFIED)

### 1. Graduation Risk Assessment
**Demo Value**: "Early warning system prevents late graduations"
- Input: Student ID
- Output: Risk level (Green/Yellow/Red) + actionable recommendation
- Algorithm: Simple math (credits per semester pace + GPA trend)
- Time Estimate: 1-2 days

### 2. Career-Based Course Recommendations  
**Demo Value**: "Students get personalized course guidance for their career goals"
- Input: Career selection (dropdown)
- Output: Recommended courses with explanations
- Algorithm: Static career-to-course mappings
- Time Estimate: 1-2 days

### 3. Prerequisite Visualization
**Demo Value**: "Students can see course dependencies clearly"
- Input: Course ID
- Output: Simple prerequisite tree/chain
- Algorithm: Hardcoded prerequisite relationships
- Time Estimate: 1 day

### 4. Career Progress Tracking
**Demo Value**: "Students track readiness for their target career"
- Input: Student ID + Career Goal
- Output: Progress percentage + next recommended course
- Algorithm: Basic completion percentage calculation
- Time Estimate: 1 day

## Technical Stack (SIMPLIFIED FOR POC)

### Backend
- **Framework**: Spring Boot (Java) - familiar and fast to develop
- **Data Storage**: Hardcoded data in classes (no database setup time)
- **API**: REST endpoints returning JSON
- **Authentication**: None (demo only)
- **Hosting**: Local development server or Heroku free tier

### Frontend
- **Technology**: Simple HTML/CSS/JavaScript (no React complexity)
- **Styling**: Bootstrap or similar for quick professional look
- **Functionality**: Basic forms and data display
- **Hosting**: Serve from Spring Boot static resources

### Mock Data
- **Students**: 10-15 sample students with varying risk levels
- **Careers**: 5 career paths (Software Engineer, Data Scientist, Teacher, Nurse, Business Analyst)
- **Courses**: 20-30 courses across different departments
- **Prerequisites**: Basic chains (CS101 → CS201 → CS301)

## Expected Deliverables

### 1. Working Application
- Runnable Spring Boot application
- All 4 core features functional
- Professional-looking UI suitable for demo
- Error-free execution during presentation

### 2. Demo-Ready Scenarios
- **Low Risk Student**: Shows green status with positive messaging
- **Medium Risk Student**: Shows yellow with actionable recommendations  
- **High Risk Student**: Shows red with urgent intervention advice
- **Career Exploration**: Multiple career paths showing different course recommendations
- **Progress Tracking**: Student at different completion levels

### 3. 5-Minute Demo Script
1. **Problem Statement** (1 min): Students graduate late, pick wrong courses
2. **Risk Assessment Demo** (1 min): Show different risk levels
3. **Career Recommendations** (1 min): Show personalized course suggestions
4. **Prerequisites** (1 min): Show course dependency visualization
5. **Progress Tracking** (1 min): Show career readiness metrics

### 4. Technical Documentation
- README with setup instructions
- API endpoint documentation
- Architecture overview (for future full implementation)
- Demo script with talking points

## Implementation Constraints

### Time Constraints
- **Total Development Time**: 3-5 days maximum
- **Demo Preparation**: 2 hours
- **Buffer Time**: Plan for 1 day of debugging/polish

### Scope Constraints
- **No Real Database**: Hardcoded data only
- **No Authentication**: Demo accounts pre-selected
- **No Real-time Updates**: Static calculations
- **No External APIs**: Mock all external data
- **No Advanced UI**: Basic but professional styling

### Quality Constraints
- **Demo Reliability**: Must work flawlessly during presentation
- **Professional Appearance**: UI should look polished, not prototype-ish
- **Clear Value Proposition**: Each feature must obviously solve a student problem
- **Scalability Story**: Architecture should clearly extend to production

## Success Criteria

### Technical Success
- [ ] All 4 features work without errors
- [ ] Application starts up quickly and reliably
- [ ] UI is responsive and professional-looking
- [ ] Demo scenarios execute smoothly
- [ ] Code is clean enough for handoff to production team

### Business Success
- [ ] Stakeholders understand the value proposition
- [ ] Clear differentiation from existing solutions
- [ ] Obvious path to production implementation
- [ ] Generates excitement for full development
- [ ] Secures approval/funding for next phase

## Risk Mitigation

### Technical Risks
- **Complexity Creep**: Stick rigidly to simplified scope
- **Demo Failures**: Test demo scenarios repeatedly
- **Setup Issues**: Document exact setup steps
- **Browser Compatibility**: Test on demo environment

### Business Risks
- **Unclear Value**: Practice explaining benefits clearly
- **Scope Questions**: Have production roadmap ready
- **Competitive Concerns**: Emphasize unique AI-driven approach
- **Cost Concerns**: Emphasize Azure integration savings

## Next Steps After POC
1. **Stakeholder Feedback**: Gather requirements for production version
2. **Architecture Planning**: Design scalable production system
3. **Team Scaling**: Plan development team structure
4. **Integration Planning**: Plan university system integrations
5. **Production Timeline**: Create realistic development schedule

## Key Messages for Demo
- **"This prevents students from graduating late"**
- **"This personalizes education like Netflix personalizes entertainment"**
- **"This leverages AI to scale academic advising"**
- **"This integrates with existing university systems"**
- **"This can help thousands of students immediately"**

