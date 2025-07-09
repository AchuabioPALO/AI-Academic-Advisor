---
mode: agent
---

# GPT-4 Academic Advisor Implementation Plan

### instructions for AI
- keep going until the job is completely solved before ending your turn
- use your tools and don't guess
- plan thoroughly before every tool call
- if you are not sure about file content or codebase structure, open them - do not hallucinate
- you MUST plan extensively before each function call, and reflect extensively on the output of the previous function call
- ALWAYS read existing files first to understand current structure before making changes
- Execute implementation phases in exact order specified
- Test each phase before moving to next
- Validate existing functionality remains intact after each change


## 🎯 Objective
Add a GPT-4 powered conversational academic advisor to the existing AI Academic Advisor POC as a modular feature that enhances but does not replace current functionality.

## 🏛️ University Context Requirements
- AI advisor must operate as "Dr. Sarah Chen" from "Metropolitan University"
- Must reference specific courses from mockData.ts (CS101, STAT201, etc.)
- Must be aware of university policies (120 credit requirement, GPA thresholds)
- Must integrate with existing risk assessment and recommendation systems
- Must maintain institutional authenticity for stakeholder demos

## 📋 Implementation Phases

### 🔍 Phase 0: Pre-Implementation Analysis (REQUIRED FIRST)
**MUST DO BEFORE ANY CODING:**
1. Read and understand existing file structure:
   - `app/lib/types.ts` - Current type definitions
   - `app/lib/mockData.ts` - Student and course data structure
   - `app/lib/academicAdvisorService.ts` - Existing service methods
   - `app/page.tsx` - Current tab structure and navigation
   - `app/globals.css` - Existing styling patterns
2. Verify existing API endpoints in `/api/` directory
3. Understand current component architecture in `/components/`
4. Document current tab IDs: ['risk', 'recommendations', 'prerequisites', 'progress', 'pathways']

### Phase 1: Foundation Setup
**Prerequisites:** Complete Phase 0 analysis

**Step 1.1: Create GPT Advisory Service**
- File: `app/lib/gptAdvisoryService.ts`
- Dependencies: Import existing types, mockData, and AcademicAdvisorService
- Required functions:
  ```typescript
  - provideAcademicAdvice(studentId: string, question: string)
  - createStudentProfileFromFreeForm(input: string)
  - getUniversityContext()
  ```

**Step 1.2: Create API Endpoint**
- File: `app/api/gpt-advisor/route.ts`
- Must handle POST requests with student context
- Error handling for missing OpenAI key
- Integration with gptAdvisoryService

**Step 1.3: Create Chat Component**
- File: `app/components/UniversityAdvisorChat.tsx`
- Props: `{ selectedStudent: string }`
- Features: Message history, typing indicator, suggested questions
- Styling: Consistent with existing components

### Phase 2: Type System Enhancement
**Prerequisites:** Phase 1 complete and tested

**Step 2.1: Extend Type Definitions**
- File: `app/lib/types.ts` (MODIFY CAREFULLY)
- Add new interfaces:
  ```typescript
  interface AdvisorMessage {
    role: 'student' | 'advisor';
    content: string;
    timestamp: Date;
  }
  
  interface AdvisorSession {
    studentId: string;
    messages: AdvisorMessage[];
    sessionId: string;
  }
  ```

**Step 2.2: Optional Mock Data Enhancement**
- File: `app/lib/mockData.ts` (OPTIONAL)
- Add sample advisor sessions for demo purposes

### Phase 3: UI Integration
**Prerequisites:** Phases 1-2 complete, all existing functionality verified intact

**Step 3.1: Update Main Page**
- File: `app/page.tsx` (CRITICAL - PRESERVE EXISTING FUNCTIONALITY)
- Current tabs array: `[risk, recommendations, prerequisites, progress, pathways]`
- ADD new tab: `{ id: 'advisor', label: 'AI Advisor Chat', icon: '🤖' }`
- Import new UniversityAdvisorChat component
- Add conditional rendering for 'advisor' tab

**Step 3.2: Add Chat Styling**
- File: `app/globals.css`
- Add chat-specific styles that match existing design system
- Ensure consistency with current component styling

### Phase 4: Advanced Features (OPTIONAL)
**Prerequisites:** Phase 3 complete, full demo testing successful

**Files to Create:**
- `app/components/AdvisorSessionHistory.tsx` - Session management
- `app/components/StudentProfileCreation.tsx` - Enhanced onboarding

## 🔧 Technical Specifications

### University Context Data Structure
```typescript
const UNIVERSITY_CONTEXT = {
  institutionName: "Metropolitan University",
  departments: ["Computer Science", "Statistics", "Business", "Education", "Mathematics", "Physics", "English", "Economics"],
  graduationRequirement: 120,
  minimumGPA: 2.0,
  maxCreditsPerSemester: 18,
  currentSemester: "Fall 2025"
};
```

### GPT-4 System Prompt Structure
```typescript
const ADVISOR_SYSTEM_PROMPT = `
You are Dr. Sarah Chen, a senior academic advisor at Metropolitan University with 15 years of experience.

INSTITUTIONAL CONTEXT:
- Work at Metropolitan University
- Students need 120 credits to graduate
- Minimum GPA requirement: 2.0
- Current semester: Fall 2025

AVAILABLE COURSES: ${mockCourses.map(c => c.code).join(', ')}
CAREER PATHS: ${mockCareers.map(c => c.title).join(', ')}

YOUR ROLE:
- Provide personalized academic guidance
- Reference specific course codes and prerequisites
- Give actionable advice based on university policies
- Maintain professional but approachable tone

STUDENT CONTEXT WILL BE PROVIDED with each request including:
- Current GPA and credits
- Completed courses
- Career goals
- Risk assessment results
- Graduate school interest
`;
```

### Required Integration Points
- **Student Data:** Use existing `mockStudents` array from `mockData.ts`
- **Risk Assessment:** Call `AcademicAdvisorService.assessGraduationRisk(studentId)`
- **Course Recommendations:** Call `AcademicAdvisorService.getCareerRecommendations(careerGoal)`
- **Graduate Programs:** Call `GraduateProgramService.getRecommendedPrograms(studentId)`
- **Progress Tracking:** Call `AcademicAdvisorService.trackCareerProgress(studentId, careerGoal)`
- **University Catalog:** Reference `mockCourses` and `mockCareers` arrays

### API Integration Points
- `/api/gpt-advisor` - New endpoint for AI advice
- Preserve ALL existing endpoints:
  - `/api/students` - Student data
  - `/api/careers` - Career information
  - `/api/courses` - Course catalog
  - `/api/course-recommendations` - Course suggestions
  - `/api/graduate-programs` - Graduate program data
  - `/api/pathways` - Career pathways
  - `/api/prerequisites` - Prerequisite chains
  - `/api/progress-tracking` - Progress data
  - `/api/risk-assessment` - Risk calculations
  - `/api/specializations` - Specialization tracks

## 🛡️ Constraints & Safeguards

### Preserve Existing Features
- **MUST NOT** modify existing component functionality
- **MUST NOT** change existing API endpoints
- **MUST NOT** alter current student data structures
- **MUST** maintain backward compatibility

### University Context Enforcement
- Always reference "Metropolitan University" 
- Use only courses from mockCourses array
- Reference specific course codes and prerequisites
- Maintain institutional persona consistency

### Demo Requirements
- Conversational interface for stakeholder presentations
- Suggested question prompts for easy demo navigation
- Real-time response with typing indicators
- Integration with existing student selection

## 📊 Success Metrics

### Functional Requirements
- [ ] AI advisor responds with university-specific context
- [ ] Integrates seamlessly with existing student profiles
- [ ] Maintains all current POC functionality
- [ ] Provides actionable academic advice
- [ ] References specific courses and programs

### Demo Requirements
- [ ] Stakeholder-ready conversational interface
- [ ] Realistic university advisor persona
- [ ] Smooth integration with existing tabs
- [ ] Professional UI/UX consistent with current design

## 🚀 Implementation Order (FOLLOW EXACTLY)

### Step-by-Step Execution Plan

**Step 1: Pre-Analysis (MANDATORY)**
1. Read `app/lib/types.ts` - understand existing interfaces
2. Read `app/lib/mockData.ts` - understand data structure
3. Read `app/lib/academicAdvisorService.ts` - understand existing methods
4. Read `app/page.tsx` lines 30-35 - verify current tabs array
5. Read `app/globals.css` - understand styling patterns
6. List all existing files in `app/api/` and `app/components/`

**Step 2: Create GPT Advisory Service**
1. Create `app/lib/gptAdvisoryService.ts`
2. Import required dependencies
3. Implement university context constants
4. Implement `provideAcademicAdvice()` method
5. Implement `createStudentProfileFromFreeForm()` method
6. Test service compiles without errors

**Step 3: Create API Endpoint**
1. Create `app/api/gpt-advisor/route.ts`
2. Implement POST handler with proper typing
3. Add error handling for missing OpenAI key
4. Add request/response validation
5. Test endpoint structure

**Step 4: Create Chat Component**
1. Create `app/components/UniversityAdvisorChat.tsx`
2. Implement message interface and state management
3. Add suggested questions feature
4. Implement typing indicator
5. Style consistent with existing components
6. Test component renders properly

**Step 5: Integrate with Main UI**
1. Read current `app/page.tsx` structure completely
2. Add import for UniversityAdvisorChat component
3. Add new tab to existing tabs array: `{ id: 'advisor', label: 'AI Advisor Chat', icon: '🤖' }`
4. Add conditional rendering for 'advisor' tab
5. VERIFY all existing tabs still work
6. Test navigation between all tabs

**Step 6: Add Styling**
1. Add chat-specific CSS to `app/globals.css`
2. Ensure consistency with existing design system
3. Test responsive design

**Step 7: Final Integration Testing**
1. Test with each student profile from mockData
2. Verify existing functionality unchanged
3. Test AI responses include university-specific context
4. Validate all tabs work correctly

## ⚠️ Critical Safety Measures

### Before ANY Changes
1. **BACKUP STRATEGY**: Read and document existing functionality
2. **INCREMENTAL CHANGES**: Implement one file at a time
3. **VALIDATION**: Test after each file creation/modification
4. **ROLLBACK PLAN**: Know how to revert if something breaks

### Red Flags - STOP Implementation If:
- Any existing API endpoint stops working
- Any existing component throws errors
- Student selection dropdown breaks
- Navigation between existing tabs fails
- TypeScript compilation errors on existing files

### Emergency Rollback
If any existing functionality breaks:
1. Remove newly created files
2. Revert modifications to existing files
3. Restore original tabs array in page.tsx
4. Test all existing functionality
5. Restart implementation from Phase 0

### Safe Modification Patterns
```typescript
// ✅ SAFE: Adding new items to arrays
const tabs = [...existingTabs, newTab];

// ✅ SAFE: Adding new properties to interfaces
interface Student {
  // ...existing properties...
  newProperty?: string; // Optional to maintain compatibility
}

// ❌ DANGEROUS: Modifying existing properties
interface Student {
  id: number; // Changed from string - BREAKS EXISTING CODE
}

// ❌ DANGEROUS: Removing or renaming existing exports
export { NewName as OldName }; // Breaks imports
```

## 🧪 Testing Strategy

### Pre-Implementation Validation
- [ ] All existing files read and understood
- [ ] Current functionality documented
- [ ] Dependencies identified and verified

### Phase-by-Phase Testing
- [ ] **Phase 1:** Service and API compile without errors
- [ ] **Phase 2:** Type definitions don't break existing code
- [ ] **Phase 3:** UI integration preserves all existing functionality
- [ ] **Phase 4:** Advanced features work independently

### Context Validation Checklist
- [ ] AI references "Metropolitan University" consistently
- [ ] AI mentions specific courses from mockCourses (CS101, STAT201, etc.)
- [ ] AI references 120 credit graduation requirement
- [ ] AI integrates risk assessment data correctly
- [ ] AI provides course-specific recommendations
- [ ] AI maintains Dr. Sarah Chen persona

### Feature Preservation Checklist
- [ ] All 5 existing tabs remain functional
- [ ] Student selection dropdown works across all tabs
- [ ] All existing API endpoints respond correctly
- [ ] No existing component functionality broken
- [ ] All existing styling preserved
- [ ] Navigation between tabs works smoothly

### Demo Readiness Checklist
- [ ] Chat interface is professional and polished
- [ ] Suggested questions provide good demo scenarios
- [ ] AI responses are contextually appropriate
- [ ] Typing indicator works smoothly
- [ ] Message history displays correctly
- [ ] Error states handled gracefully

## 🔒 Environment Setup & Dependencies

### Required Environment Variables
```bash
# Add to .env.local (create if doesn't exist)
OPENAI_API_KEY=your_gpt4_api_key_here
```

### Package Dependencies
Current project uses Next.js with TypeScript. No new dependencies required for basic implementation.

Optional enhancement packages:
```bash
# For enhanced chat features (Phase 4 only)
npm install react-markdown  # For rich text formatting
npm install lucide-react    # For additional icons
```

### File Dependencies Map
```
app/lib/gptAdvisoryService.ts
├── IMPORTS: types.ts, mockData.ts, academicAdvisorService.ts
├── USED BY: api/gpt-advisor/route.ts

app/api/gpt-advisor/route.ts
├── IMPORTS: gptAdvisoryService.ts
├── USED BY: components/UniversityAdvisorChat.tsx

app/components/UniversityAdvisorChat.tsx
├── IMPORTS: types.ts (new AdvisorMessage interface)
├── USED BY: page.tsx

app/page.tsx (MODIFY CAREFULLY)
├── IMPORTS: UniversityAdvisorChat.tsx
├── MODIFIES: tabs array, conditional rendering
```

## 📝 Documentation Updates
- Update README.md with new AI advisor feature
- Document API endpoints in existing format
- Add component documentation
- Update demo script with AI advisor scenarios

---

**Expected Deliverable:** Fully functional GPT-4 academic advisor integrated as a new tab in the existing POC, maintaining all current functionality while adding conversational AI capabilities contextualized to Metropolitan University.