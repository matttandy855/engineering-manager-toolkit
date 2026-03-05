# Sprint Planner Skill

*"Intelligent sprint planning with capacity modeling and velocity forecasting"*

A specialized skill for automated sprint planning that balances team capacity, velocity trends, and stakeholder priorities to create optimal sprint plans.

## Overview

The Sprint Planner skill combines historical team data, current capacity constraints, and backlog priorities to generate realistic sprint plans with risk assessment and confidence intervals.

## Inputs

- **Team Velocity History**: Past sprint completion data (6+ sprints recommended)
- **Current Backlog**: Prioritized list of stories with estimates
- **Team Capacity**: Available hours accounting for PTO, meetings, and other commitments
- **Sprint Configuration**: Sprint length, team size, and working patterns

## Outputs

- **Sprint Plan**: Recommended story allocation with capacity utilization
- **Risk Assessment**: Identified delivery risks and mitigation strategies
- **Confidence Score**: Probability of successful sprint completion
- **Alternative Scenarios**: Stretch and conservative sprint options

## Usage

### Basic Sprint Planning
```bash
# Generate sprint plan for a team
sprint-planner execute \
  --team "platform-engineering" \
  --sprint-length 2 \
  --capacity-hours 320 \
  --backlog-source "linear://project/PLAT" \
  --confidence-target 0.8
```

### Advanced Configuration
```bash
# Sprint planning with custom parameters
sprint-planner execute \
  --config configs/platform-team.yaml \
  --include-tech-debt-budget 20 \
  --buffer-percentage 15 \
  --risk-tolerance medium \
  --output-format json
```

## Configuration

### Team Profile
```yaml
# configs/platform-team.yaml
team:
  name: "Platform Engineering"
  size: 8
  sprint_length_weeks: 2
  
  velocity_config:
    baseline_calculation: "average_6_sprints"
    seasonal_adjustment: true
    exclude_holidays: true
    confidence_level: 0.8
  
  capacity_planning:
    working_hours_per_day: 8
    meeting_overhead_percentage: 20
    context_switching_buffer: 15
    oncall_reduction_percentage: 25
  
  planning_preferences:
    tech_debt_budget_percentage: 20
    innovation_time_percentage: 10
    bug_fix_priority: "high"
    cross_team_dependency_buffer: 10
```

### Risk Thresholds
```yaml
risk_assessment:
  delivery_risk:
    velocity_variance_threshold: 0.3
    capacity_utilization_max: 0.85
    dependency_count_warning: 3
    
  quality_risk:
    technical_debt_ratio_max: 0.25
    bug_backlog_size_warning: 10
    test_coverage_min: 0.8
    
  team_risk:
    new_team_member_impact: 0.15
    key_person_dependency: true
    skill_gap_assessment: true
```

## Algorithm Details

### Velocity Calculation
```javascript
class VelocityCalculator {
  calculateBaselineVelocity(sprintHistory, config) {
    const relevantSprints = this.filterSprints(sprintHistory, config);
    
    switch (config.baseline_calculation) {
      case 'average_6_sprints':
        return this.weightedAverage(relevantSprints, 6);
      case 'trend_analysis':
        return this.trendBasedVelocity(relevantSprints);
      case 'monte_carlo':
        return this.monteCarloSimulation(relevantSprints);
    }
  }
  
  applySeasonalAdjustments(velocity, currentDate) {
    const seasonalFactors = {
      'holiday_season': 0.8,
      'summer_vacation': 0.9,
      'conference_season': 0.85,
      'year_end': 0.75
    };
    
    return velocity * this.getSeasonalFactor(currentDate, seasonalFactors);
  }
  
  calculateConfidenceInterval(sprintHistory, velocity) {
    const variance = this.calculateVariance(sprintHistory);
    const standardDeviation = Math.sqrt(variance);
    
    return {
      lower_bound: velocity - (1.96 * standardDeviation),
      upper_bound: velocity + (1.96 * standardDeviation),
      confidence_level: 0.95
    };
  }
}
```

### Capacity Modeling
```javascript
class CapacityModeler {
  calculateAvailableCapacity(team, sprintDates, config) {
    const totalHours = this.calculateTotalHours(team.size, sprintDates, config);
    const adjustments = this.calculateAdjustments(team, sprintDates, config);
    
    return {
      total_hours: totalHours,
      available_hours: totalHours - adjustments.total,
      adjustments: adjustments,
      utilization_target: config.capacity_planning.utilization_target || 0.8
    };
  }
  
  calculateAdjustments(team, sprintDates, config) {
    return {
      pto_hours: this.calculatePTOImpact(team, sprintDates),
      meeting_overhead: this.calculateMeetingOverhead(team, config),
      context_switching: this.calculateContextSwitchingCost(team, config),
      oncall_duties: this.calculateOncallImpact(team, sprintDates, config),
      learning_time: this.calculateLearningTime(team, config),
      buffer_hours: this.calculatePlanningBuffer(team, config)
    };
  }
}
```

### Story Assignment Algorithm
```javascript
class StoryAssignmentEngine {
  generateOptimalAssignment(backlog, capacity, velocity, constraints) {
    // Multi-objective optimization considering:
    // 1. Capacity utilization
    // 2. Priority ordering
    // 3. Skill matching
    // 4. Dependency management
    
    const candidates = this.generateCandidateAssignments(backlog, capacity);
    const scoredCandidates = candidates.map(assignment => ({
      assignment,
      score: this.scoreAssignment(assignment, constraints)
    }));
    
    const optimal = this.selectOptimalAssignment(scoredCandidates);
    
    return {
      primary_plan: optimal,
      alternative_plans: this.generateAlternatives(optimal, candidates),
      risk_assessment: this.assessPlanRisk(optimal, constraints)
    };
  }
  
  scoreAssignment(assignment, constraints) {
    const scores = {
      priority_alignment: this.scorePriorityAlignment(assignment),
      capacity_utilization: this.scoreCapacityUtilization(assignment),
      skill_match: this.scoreSkillMatch(assignment),
      dependency_risk: this.scoreDependencyRisk(assignment),
      team_balance: this.scoreTeamBalance(assignment)
    };
    
    return this.weightedScore(scores, constraints.scoring_weights);
  }
}
```

## Risk Assessment Framework

### Delivery Risk Analysis
```javascript
class RiskAssessment {
  assessDeliveryRisk(sprintPlan, teamContext) {
    const risks = [];
    
    // Capacity overcommitment
    if (sprintPlan.capacity_utilization > 0.9) {
      risks.push({
        type: 'capacity_overcommitment',
        severity: 'high',
        probability: 0.8,
        impact: 'sprint_goals_at_risk',
        mitigation: 'Reduce scope or extend timeline'
      });
    }
    
    // Velocity variance
    const velocityVariance = this.calculateVelocityVariance(teamContext);
    if (velocityVariance > 0.3) {
      risks.push({
        type: 'velocity_unpredictability',
        severity: 'medium',
        probability: 0.6,
        impact: 'delivery_date_uncertainty',
        mitigation: 'Build larger planning buffer'
      });
    }
    
    // Dependency risks
    const externalDependencies = this.identifyExternalDependencies(sprintPlan);
    if (externalDependencies.length > 2) {
      risks.push({
        type: 'external_dependencies',
        severity: 'medium',
        probability: 0.4 * externalDependencies.length,
        impact: 'potential_delays',
        mitigation: 'Proactive dependency management'
      });
    }
    
    return this.prioritizeRisks(risks);
  }
  
  generateMitigationPlan(risks) {
    return risks.map(risk => ({
      risk_id: risk.id,
      mitigation_strategy: this.selectMitigationStrategy(risk),
      implementation_steps: this.generateMitigationSteps(risk),
      success_criteria: this.defineMitigationSuccessCriteria(risk),
      monitoring_plan: this.createRiskMonitoringPlan(risk)
    }));
  }
}
```

## Output Examples

### Sprint Plan Output
```json
{
  "sprint_plan": {
    "sprint_id": "PLAT-Sprint-48",
    "dates": {
      "start": "2026-03-10",
      "end": "2026-03-21"
    },
    "team_capacity": {
      "total_hours": 320,
      "available_hours": 256,
      "utilization_target": 0.8,
      "planned_utilization": 0.78
    },
    "story_allocation": [
      {
        "story_id": "PLAT-1234",
        "title": "Implement OAuth2 flow",
        "estimate": 8,
        "priority": "high",
        "assignee": "alex.chen",
        "dependencies": []
      },
      {
        "story_id": "PLAT-1235", 
        "title": "Database migration framework",
        "estimate": 13,
        "priority": "high",
        "assignee": "morgan.taylor",
        "dependencies": ["PLAT-1234"]
      }
    ],
    "confidence_metrics": {
      "completion_probability": 0.85,
      "velocity_confidence": 0.82,
      "capacity_confidence": 0.91
    }
  },
  "risk_assessment": {
    "overall_risk": "medium",
    "identified_risks": [
      {
        "type": "skill_gap",
        "description": "OAuth2 implementation requires security expertise",
        "mitigation": "Pair programming with senior engineer",
        "probability": 0.3
      }
    ]
  },
  "alternative_plans": {
    "conservative": {
      "story_count": 6,
      "completion_probability": 0.95
    },
    "stretch": {
      "story_count": 10,
      "completion_probability": 0.65
    }
  }
}
```

## Success Metrics

### Planning Accuracy
- **Velocity Prediction Error**: <15% variance from actual
- **Scope Completion Rate**: >85% of planned stories completed
- **Capacity Utilization Accuracy**: Within 10% of planned utilization

### Team Satisfaction
- **Planning Meeting Duration**: <2 hours for 2-week sprint
- **Team Confidence in Plan**: >8/10 average rating
- **Planning Process Satisfaction**: >8.5/10 rating

### Process Improvement
- **Planning Time Reduction**: 50% less time than manual planning
- **Risk Prediction Accuracy**: >80% of flagged risks materialize
- **Mitigation Effectiveness**: >70% of risks successfully mitigated

## Dependencies

### Required Data Sources
- **Project Management**: Jira, Linear, Azure DevOps APIs
- **Team Calendar**: Google Calendar, Outlook for PTO tracking
- **Historical Data**: At least 6 sprints of velocity data
- **Team Configuration**: Skills, seniority, working patterns

### Optional Enhancements
- **Code Quality Metrics**: SonarQube, CodeClimate for complexity analysis
- **Deployment Data**: CI/CD pipeline success rates
- **Customer Feedback**: Support ticket correlation with sprint deliveries

The Sprint Planner skill transforms manual, gut-feeling-based sprint planning into a data-driven, optimized process that consistently delivers predictable results while maintaining team confidence and stakeholder trust.