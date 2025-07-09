---
mode: agent
---

# University Data Integration POC Enhancement

### instructions for AI
- keep going until the job is completely solved before ending your turn
- use your tools and dont guess
- plan throroughly before every tool call
- if you are not sure about file content or codebase structure , open them- do not hallucinate
- you MUST plan extensively before each function call, and reflect extensively on the output of the previous function call

## Objective
Enhance the existing AI Academic Advisor POC by integrating graduate program pathways and specialized program options for students already enrolled at the fictional university. This includes post-graduation opportunities (graduate school, specialized programs, international exchange) and internal program transfers. The integration should remain POC-appropriate with anonymized university references in the UI.

## Expected Output
- Enhanced type definitions for graduate programs and advanced pathways
- New mock data with realistic post-graduation and specialized program information
- API endpoints for graduate program and pathway queries
- UI components for displaying program pathways and eligibility
- Updated career paths with graduate program and specialization mappings
- Academic pathway progression and qualification logic simulation

## Constraints
- Maintain POC simplicity - avoid over-engineering
- No specific university names in UI (use generic terms like "International Tech University", "Engineering Institute", etc.)
- Keep all data as mock/demonstration data
- Preserve existing POC functionality
- Focus on demonstrating concepts rather than production-ready features

### Instructions for AI
- Keep going until the job is completely solved before ending your turn
- Use your tools and don't guess
- Plan thoroughly before every tool call
- If you are not sure about file content or codebase structure, open them - do not hallucinate
- You MUST plan extensively before each function call, and reflect extensively on the output of the previous function call

## Execution Plan

### Phase 1: Type System Enhancement
**Goal**: Extend existing types to support graduate programs and advanced academic pathways

**Tasks**:
1. **Analyze current types.ts structure**
   - Review existing Student, Career, Course interfaces
   - Understand current data relationships for enrolled students
   - Note existing export structure and dependencies

2. **Add new interfaces** (append to types.ts):
   ```typescript
   export interface GraduateProgram {
     id: string;
     title: string;
     degree: 'MS' | 'PhD';
     department: string;
     description: string;
     duration: string;
     minGPA: number;
     requiredCourses: string[];
     applicationDeadline: string;
     startDate: string;
     anonymizedPartner?: string; // Generic partner names
   }

   export interface SpecializationTrack {
     id: string;
     name: string;
     department: string;
     additionalCourses: string[];
     minGPA: number;
     careerOutcomes: string[];
     description: string;
   }

   export interface PostGradPathway {
     id: string;
     type: 'graduate' | 'exchange' | 'internship';
     title: string;
     description: string;
     eligibilityRequirements: string[];
     timeline: string;
     benefits: string[];
   }

   export interface EligibilityCheck {
     isEligible: boolean;
     missingRequirements: string[];
     admissionChance: 'High' | 'Medium' | 'Low';
     recommendations: string[];
   }
   ```

3. **Extend existing interfaces** (modify existing types):
   - Add `graduatePrograms?: GraduateProgram[]` to Career interface
   - Add `specializations?: string[]` to Student interface
   - Add `postGradInterest?: string[]` to Student interface
   - Add `interestedInGradSchool?: boolean` to Student interface

**Success Criteria**: Types compile without errors and support graduate/specialized program pathways

**Implementation Notes**:
- Keep all new fields optional to maintain backward compatibility
- Use consistent naming conventions with existing types
- Ensure anonymized partner names for any external references

### Phase 2: Mock Data Enhancement  
**Goal**: Add realistic graduate and specialized program data for current university students

**Tasks**:
1. **Create mock graduate programs** (append to mockData.ts):
   - Add `mockGraduatePrograms` array with 8-10 programs
   - MS in Computer Science, MS in Data Science, PhD in Engineering, etc.
   - Use generic names like "Advanced Computing Institute", "Metropolitan Tech Graduate School"
   - Include realistic admission requirements (3.0+ GPA, specific courses)
   - Application deadlines and start dates for demo scenarios

2. **Create mock specialization tracks** (append to mockData.ts):
   - Add `mockSpecializationTracks` array with 6-8 tracks
   - AI/Robotics track for CS students
   - Data Analytics track for Business students
   - Include additional course requirements and career outcomes
   - Map to existing career paths

3. **Update existing student data** (modify mockStudents):
   - Add `interestedInGradSchool: true/false` to each student
   - Add `postGradInterest: ['graduate-school', 'industry', 'research']` arrays
   - Ensure Alice Johnson (high achiever) has graduate school interest
   - Ensure Bob Smith (struggling) has different pathway interests

4. **Update existing career data** (modify mockCareers):
   - Add `graduatePrograms` array to each career
   - Link software-engineer to CS graduate programs
   - Link data-scientist to Data Science programs
   - Include progression pathways from undergrad to grad

5. **Create post-grad pathway data** (append to mockData.ts):
   - Add `mockPostGradPathways` array
   - Include study abroad, research internships, industry partnerships
   - Use anonymized partner names: "International Engineering Consortium"

**Success Criteria**: Rich mock data that demonstrates progression pathways without breaking existing functionality

**Implementation Notes**:
- Maintain existing mockStudents structure, only add new optional fields
- Ensure new data references existing course IDs (CS101, MATH101, etc.)
- Use generic institution names consistently
- Keep data realistic but demo-appropriate (not too complex)

### Phase 3: Service Layer Development
**Goal**: Create services to handle graduate program and pathway logic

**Tasks**:
1. **Create GraduateProgramService** (new file: app/lib/graduateProgramService.ts):
   - Import necessary types and mock data
   - `checkEligibility(student: Student, program: GraduateProgram): EligibilityCheck`
   - `calculateAdmissionChance(student: Student, program: GraduateProgram): string`
   - `getRecommendedPrograms(studentId: string): GraduateProgram[]`
   - `getApplicationTimeline(program: GraduateProgram): string[]`
   - Use existing patterns from academicAdvisorService.ts

2. **Create SpecializationService** (new file: app/lib/specializationService.ts):
   - `getAvailableSpecializations(studentMajor: string): SpecializationTrack[]`
   - `checkSpecializationEligibility(student: Student, track: SpecializationTrack): boolean`
   - `getSpecializationProgress(student: Student, trackId: string): number`

3. **Enhance existing AcademicAdvisorService**:
   - Add `getGraduatePathwayRecommendations(studentId: string)` method
   - Integrate graduate planning into existing risk assessment
   - Add graduate prep courses to course recommendations
   - Maintain existing method signatures and functionality

**Success Criteria**: Clean service layer with graduate program logic that integrates with existing services

**Implementation Notes**:
- Follow existing service patterns in academicAdvisorService.ts
- Import from mock data arrays created in Phase 2
- Use static methods to maintain consistency
- Handle edge cases (student not found, etc.) like existing services

### Phase 4: API Endpoint Development
**Goal**: Create endpoints to serve graduate program and pathway data

**Tasks**:
1. **Create `/api/graduate-programs/route.ts`**:
   - Follow existing API pattern from app/api/students/route.ts
   - Accept `?studentId=STU001` query parameter
   - Return graduate programs with eligibility status using GraduateProgramService
   - Include error handling and 404 responses for invalid students
   - Return JSON format consistent with existing APIs

2. **Create `/api/specializations/route.ts`**:
   - Accept `?studentId=STU001&major=Computer Science` parameters
   - Return available specialization tracks using SpecializationService
   - Include prerequisite and progress information

3. **Enhance existing endpoints** (modify existing files):
   - Update `/api/course-recommendations/route.ts`: Add graduate prep course logic
   - Update `/api/progress-tracking/route.ts`: Include graduate program milestones
   - Update `/api/risk-assessment/route.ts`: Factor in graduate school planning
   - Maintain existing functionality while adding new features

**Success Criteria**: RESTful APIs that work with existing frontend components and new graduate features

**Implementation Notes**:
- Use exact same structure as existing route.ts files
- Import services created in Phase 3
- Follow existing error handling patterns
- Test with existing student IDs (STU001, STU002, etc.)
- Maintain backward compatibility with existing API consumers

### Phase 5: UI Component Development
**Goal**: Create components to display graduate program and pathway information

**Tasks**:
1. **Create GraduatePrograms component** (app/components/GraduatePrograms.tsx):
   - Follow existing component patterns from CourseRecommendations.tsx
   - Use same styling classes and structure
   - Display available graduate programs with eligibility indicators
   - Include application deadlines and requirements
   - Add student selector dropdown like existing components
   - Show admission chance with color-coded indicators

2. **Create SpecializationTracks component** (app/components/SpecializationTracks.tsx):
   - Visual pathway selector using existing UI patterns
   - Show required additional courses for each track
   - Display career outcomes and benefits
   - Use card-based layout consistent with existing design

3. **Create PostGradPathways component** (app/components/PostGradPathways.tsx):
   - Compare different post-graduation options
   - Timeline view from current status to post-grad goals
   - Include study abroad and research opportunities
   - Use existing progress bar and card components

4. **Update existing components** (modify existing files):
   - CourseRecommendations.tsx: Add graduate prep course section
   - ProgressTracking.tsx: Include graduate program milestone progress
   - RiskAssessment.tsx: Add post-graduation planning factor
   - Maintain all existing functionality while adding new sections

**Success Criteria**: Responsive UI components that match existing design and integrate seamlessly

**Implementation Notes**:
- Copy styling patterns from existing components exactly
- Use same state management patterns (useState, useEffect)
- Follow existing API calling patterns with fetch()
- Maintain existing student selector functionality
- Use consistent color scheme and typography
- Test with existing student data (Alice, Bob, Carol, etc.)

### Phase 6: Integration and Enhancement
**Goal**: Integrate new features into main application

**Tasks**:
1. **Add new tab to main navigation** (modify app/page.tsx):
   - Add 'pathways' tab to existing tabs array: `{ id: 'pathways', label: 'Graduate Pathways', icon: '🎓' }`
   - Import new components: GraduatePrograms, SpecializationTracks, PostGradPathways
   - Add pathways case to existing switch statement in renderContent function
   - Maintain existing state management and data fetching patterns

2. **Create pathways tab content**:
   - Use existing tab structure with sub-navigation for pathways features
   - Include Graduate Programs, Specializations, and Post-Grad options
   - Maintain consistent styling with existing tabs

3. **Cross-component integration**:
   - Ensure new APIs work with existing student/career selection
   - Link course recommendations to graduate program requirements
   - Connect career goals to graduate program options
   - Use existing student and career state from main app

4. **Enhanced demo scenarios** (update existing student data):
   - Alice Johnson: High GPA, interested in MS Computer Science
   - Bob Smith: Struggling student, consider alternative pathways
   - Carol Davis: Excellent student, PhD research track
   - David Wilson: Business student, MBA pathway
   - Emma Brown: Education major, graduate certification programs

**Success Criteria**: Seamless integration with existing tab navigation and student selection

**Implementation Notes**:
- Follow exact pattern of existing tabs in page.tsx
- Maintain existing useState and useEffect patterns
- Use existing students and careers state variables
- Don't break existing tab functionality
- Test all existing tabs still work after integration
- Ensure new pathways tab loads correctly with student data

### Phase 7: Testing and Refinement
**Goal**: Ensure robust POC functionality

**Tasks**:
1. **Functionality testing**:
   - Test all new API endpoints with existing student IDs (STU001-STU005)
   - Verify UI component interactions with existing student selector
   - Check data flow between new and existing components
   - Ensure existing tabs (Risk, Recommendations, Prerequisites, Progress) still work
   - Test new Graduate Pathways tab with all students

2. **Demo scenario validation**:
   - Walk through Alice Johnson (high achiever) → graduate school pathway
   - Walk through Bob Smith (struggling) → alternative pathway options  
   - Walk through Carol Davis (excellent) → PhD research track
   - Verify realistic and compelling narratives for each student type
   - Ensure no specific university names appear in UI (use generic terms only)

3. **Performance and compatibility**:
   - Ensure smooth UI interactions between tabs
   - Check API response times are reasonable for POC
   - Verify existing functionality remains unchanged
   - Test responsive design on different screen sizes

4. **Documentation updates**:
   - Update POC completion summary with new graduate pathway features
   - Document new API endpoints and their usage
   - Update demo script to include graduate pathway scenarios
   - Add feature descriptions to existing documentation

**Success Criteria**: Polished POC with enhanced graduate pathway features that don't break existing functionality

**Implementation Notes**:
- Test with browser dev tools to check for console errors
- Verify all existing API endpoints still return expected data
- Check that existing student/career selection still works across all tabs
- Ensure new mock data integrates properly with existing data
- Validate that all generic university names are used consistently

## Pre-Execution Checklist

### Before Starting Implementation:
1. **Verify Current POC State**:
   - Confirm all existing features work (Risk, Recommendations, Prerequisites, Progress)
   - Check that student data loads correctly (STU001-STU005)
   - Verify API endpoints return expected data (/api/students, /api/careers, etc.)
   - Test existing components render without errors

2. **Existing File Structure to Preserve**:
   - `app/lib/types.ts` - extend, don't replace
   - `app/lib/mockData.ts` - append new data, don't modify existing
   - `app/lib/academicAdvisorService.ts` - enhance existing methods
   - `app/page.tsx` - add new tab to existing tabs array
   - All existing `app/api/*/route.ts` files - enhance, don't break

3. **Critical Constraints**:
   - ALL existing functionality must continue working
   - Use existing student IDs (STU001, STU002, etc.) in new features
   - Match existing UI patterns and styling exactly
   - Use generic university names only (no specific institutions)
   - Keep changes additive, not destructive

4. **Testing Strategy**:
   - Test each phase incrementally
   - Verify existing features after each change
   - Use browser dev tools to check for errors
   - Test with all 5 existing students in mock data

## Key Implementation Notes

### Data Anonymization Strategy
- Use generic university names: "International Tech University", "Global Engineering Institute", "Metropolitan University"
- Keep program structures realistic but not identifiable
- Focus on demonstrating concepts rather than specific institutions

### POC Boundaries
- Maintain simplicity - this is a demonstration, not a production system
- Use static mock data rather than live integrations
- Focus on showing algorithmic capabilities and user experience value
- Keep implementation lightweight and maintainable

### Technical Considerations
- Preserve existing POC architecture and patterns
- Ensure new features don't break current functionality  
- Maintain consistent code style and component patterns
- Use TypeScript effectively for type safety

### Demo Value Proposition
- Show how AI can guide enrolled students through graduate school planning and specialization choices
- Demonstrate intelligent matching between undergraduate performance and graduate program eligibility
- Highlight value of early post-graduation planning and career pathway optimization
- Showcase internal program transfer and specialization track guidance for current students

## Success Metrics
- All new features functional and integrated
- No breaking changes to existing POC features
- Clean, maintainable code that follows existing patterns
- Compelling demo scenarios that show clear value
- No specific university branding in user-facing elements
- Enhanced POC completion summary reflecting new capabilities
