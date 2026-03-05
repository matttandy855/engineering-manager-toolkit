# Technical Debt Auditor Skill

*"Automated technical debt assessment, prioritization, and remediation planning"*

A comprehensive skill for identifying, quantifying, and prioritizing technical debt across codebases to enable data-driven technical debt management decisions.

## Overview

The Technical Debt Auditor analyzes code quality, architecture patterns, and development velocity to provide actionable insights on technical debt impact and prioritize remediation efforts based on business value and engineering efficiency.

## Inputs

- **Codebase Access**: Repository URLs and branch information
- **Development Metrics**: PR velocity, bug rates, feature delivery speed
- **Architecture Documentation**: System design docs, API specifications
- **Business Context**: Feature priorities, team capacity, delivery timelines

## Outputs

- **Debt Assessment Report**: Quantified technical debt analysis
- **Prioritization Matrix**: ROI-based remediation priority ranking  
- **Remediation Roadmap**: Phased approach to debt reduction
- **Impact Forecasting**: Predicted velocity improvements from debt reduction

## Usage

### Basic Debt Analysis
```bash
# Analyze technical debt in a repository
tech-debt-auditor execute \
  --repo "https://github.com/company/main-app" \
  --frameworks "React,Node.js,PostgreSQL" \
  --business-impact-weight 0.4 \
  --velocity-impact-weight 0.6
```

### Comprehensive Audit
```bash
# Full technical debt audit with business context
tech-debt-auditor execute \
  --config configs/platform-audit.yaml \
  --include-security-debt \
  --include-performance-debt \
  --include-maintainability-debt \
  --remediation-budget 40 \
  --output detailed-report
```

## Configuration

### Analysis Parameters
```yaml
# configs/platform-audit.yaml
analysis_scope:
  repositories:
    - url: "https://github.com/company/api-service"
      weight: 0.4  # Business criticality weight
    - url: "https://github.com/company/web-app" 
      weight: 0.3
    - url: "https://github.com/company/mobile-api"
      weight: 0.3
  
  frameworks:
    - React
    - Node.js
    - PostgreSQL
    - Redis
    - AWS Lambda

debt_categories:
  code_quality:
    cyclomatic_complexity_threshold: 10
    duplication_percentage_threshold: 5
    test_coverage_minimum: 80
    
  architecture:
    module_coupling_threshold: 0.7
    dependency_freshness_days: 180
    api_consistency_score_minimum: 0.8
    
  security:
    vulnerability_age_threshold_days: 30
    dependency_security_scan: true
    code_security_patterns: true
    
  performance:
    bundle_size_threshold_mb: 5
    api_response_time_threshold_ms: 500
    database_query_optimization: true
```

### Business Impact Modeling
```yaml
business_context:
  team_configuration:
    senior_engineers: 3
    mid_engineers: 4
    junior_engineers: 2
    hourly_cost_average: 75
    
  delivery_constraints:
    sprint_length_weeks: 2
    velocity_target_story_points: 40
    quality_gate_requirements: "high"
    
  feature_priorities:
    new_feature_development: 0.6
    bug_fixes: 0.2
    technical_debt: 0.15
    maintenance: 0.05
```

## Analysis Framework

### Code Quality Assessment
```javascript
class CodeQualityAnalyzer {
  async analyzeCodebase(repository) {
    const metrics = await this.gatherCodeMetrics(repository);
    
    return {
      complexity_analysis: this.analyzeCyclomaticComplexity(metrics),
      duplication_analysis: this.analyzeDuplication(metrics),
      coverage_analysis: this.analyzeTestCoverage(metrics),
      maintainability_index: this.calculateMaintainabilityIndex(metrics),
      hotspot_identification: this.identifyProblemAreas(metrics)
    };
  }
  
  calculateTechnicalDebtHours(complexityScore, duplicationScore, coverageScore) {
    // Empirical formula based on industry research
    const complexityHours = (complexityScore - 10) * 0.5; // Hours per complexity point above threshold
    const duplicationHours = duplicationScore * 0.3; // Hours per percentage of duplication
    const coverageHours = (80 - coverageScore) * 0.2; // Hours per percentage below 80% coverage
    
    return Math.max(0, complexityHours + duplicationHours + coverageHours);
  }
  
  identifyRefactoringOpportunities(metrics) {
    return {
      extract_method_candidates: this.findLongMethods(metrics),
      extract_class_candidates: this.findLargeClasses(metrics),
      eliminate_duplication: this.findDuplicatedCode(metrics),
      simplify_conditionals: this.findComplexConditionals(metrics)
    };
  }
}
```

### Architecture Debt Detection
```javascript
class ArchitectureAnalyzer {
  async assessArchitecturalDebt(codebase, documentation) {
    const dependencyAnalysis = await this.analyzeDependencies(codebase);
    const layeringAnalysis = await this.analyzeLayering(codebase);
    const couplingAnalysis = await this.analyzeCoupling(codebase);
    
    return {
      dependency_debt: this.assessDependencyDebt(dependencyAnalysis),
      layering_violations: this.identifyLayeringViolations(layeringAnalysis),
      coupling_issues: this.identifyCouplingIssues(couplingAnalysis),
      architectural_consistency: this.assessConsistency(codebase, documentation)
    };
  }
  
  calculateArchitecturalRefactoringEffort(violations) {
    const effortMapping = {
      'circular_dependency': 8, // hours
      'layer_violation': 4,
      'tight_coupling': 6,
      'missing_abstraction': 12,
      'god_class': 16
    };
    
    return violations.reduce((total, violation) => {
      return total + (effortMapping[violation.type] || 4) * violation.severity;
    }, 0);
  }
}
```

### Business Impact Calculator
```javascript
class BusinessImpactCalculator {
  calculateVelocityImpact(technicalDebt, teamMetrics) {
    // Model based on empirical research showing relationship between 
    // technical debt and development velocity
    
    const debtRatio = technicalDebt.total_hours / teamMetrics.total_code_hours;
    const velocityDragFactor = this.calculateVelocityDrag(debtRatio);
    
    return {
      current_velocity_drag: velocityDragFactor,
      potential_velocity_gain: this.calculatePotentialGain(velocityDragFactor),
      feature_delivery_impact: this.modelFeatureDeliveryImpact(velocityDragFactor),
      bug_rate_correlation: this.calculateBugRateImpact(debtRatio)
    };
  }
  
  calculateROI(debtItem, remediationCost, businessContext) {
    const velocityGain = this.calculateVelocityGain(debtItem);
    const qualityImpact = this.calculateQualityImpact(debtItem);
    const maintenanceSavings = this.calculateMaintenanceSavings(debtItem);
    
    const annualBenefit = 
      (velocityGain * businessContext.feature_value_per_sprint * 26) + // 26 sprints per year
      (qualityImpact * businessContext.bug_cost_average * 52) + // Weekly bug impact
      maintenanceSavings;
    
    return {
      roi_percentage: (annualBenefit / remediationCost) * 100,
      payback_period_weeks: remediationCost / (annualBenefit / 52),
      net_present_value: this.calculateNPV(annualBenefit, remediationCost, 3), // 3-year horizon
      confidence_level: this.calculateConfidence(debtItem.metrics)
    };
  }
}
```

## Debt Classification System

### Debt Categories
```yaml
debt_types:
  code_quality_debt:
    description: "Poor code structure, complexity, duplication"
    indicators:
      - high_cyclomatic_complexity
      - code_duplication
      - low_test_coverage
      - poor_naming_conventions
    impact: "Slower feature development, higher bug rate"
    
  architectural_debt:
    description: "Design decisions that impede future development"
    indicators:
      - layer_violations
      - tight_coupling
      - missing_abstractions
      - circular_dependencies
    impact: "Reduced system flexibility, harder to scale"
    
  security_debt:
    description: "Known security vulnerabilities and weak patterns"
    indicators:
      - outdated_dependencies
      - security_vulnerabilities
      - weak_authentication
      - insufficient_encryption
    impact: "Security risks, compliance violations"
    
  performance_debt:
    description: "Performance issues that impact user experience"
    indicators:
      - slow_queries
      - memory_leaks
      - inefficient_algorithms
      - large_bundle_sizes
    impact: "Poor user experience, infrastructure costs"
    
  documentation_debt:
    description: "Missing or outdated documentation"
    indicators:
      - missing_api_docs
      - outdated_architecture_docs
      - no_code_comments
      - missing_runbooks
    impact: "Slower onboarding, knowledge silos"
```

### Prioritization Matrix
```javascript
class DebtPrioritizer {
  generatePriorityMatrix(debtItems, businessContext) {
    return debtItems.map(item => {
      const impactScore = this.calculateImpactScore(item);
      const effortScore = this.calculateEffortScore(item);
      const riskScore = this.calculateRiskScore(item);
      
      return {
        ...item,
        priority_score: this.calculatePriorityScore(impactScore, effortScore, riskScore),
        quadrant: this.assignQuadrant(impactScore, effortScore),
        recommended_timeline: this.recommendTimeline(impactScore, riskScore),
        resource_requirements: this.calculateResourceRequirements(effortScore)
      };
    }).sort((a, b) => b.priority_score - a.priority_score);
  }
  
  assignQuadrant(impact, effort) {
    if (impact >= 7 && effort <= 5) return "quick_wins";
    if (impact >= 7 && effort > 5) return "major_projects";
    if (impact < 7 && effort <= 5) return "fill_ins";
    return "thankless_tasks";
  }
  
  recommendTimeline(impact, risk) {
    if (impact >= 8 || risk >= 8) return "immediate";
    if (impact >= 6 || risk >= 6) return "next_quarter";
    return "backlog";
  }
}
```

## Remediation Planning

### Phased Remediation Strategy
```javascript
class RemediationPlanner {
  generateRemediationRoadmap(prioritizedDebt, teamCapacity, businessConstraints) {
    const phases = this.createPhases(prioritizedDebt, teamCapacity);
    
    return {
      roadmap_overview: this.generateOverview(phases),
      phase_details: phases.map(phase => this.generatePhaseDetails(phase)),
      success_metrics: this.defineSuccessMetrics(prioritizedDebt),
      risk_mitigation: this.planRiskMitigation(phases),
      resource_allocation: this.optimizeResourceAllocation(phases, teamCapacity)
    };
  }
  
  createPhases(debtItems, capacity) {
    const phases = [];
    let currentPhase = { items: [], total_effort: 0, timeline: "Q1" };
    
    for (const item of debtItems) {
      if (currentPhase.total_effort + item.effort_hours > capacity.quarterly_hours) {
        phases.push(currentPhase);
        currentPhase = { items: [item], total_effort: item.effort_hours, timeline: this.getNextQuarter() };
      } else {
        currentPhase.items.push(item);
        currentPhase.total_effort += item.effort_hours;
      }
    }
    
    if (currentPhase.items.length > 0) phases.push(currentPhase);
    return phases;
  }
}
```

## Output Examples

### Technical Debt Assessment Report
```json
{
  "assessment_summary": {
    "total_debt_hours": 1250,
    "debt_ratio": 0.23,
    "velocity_impact": -15,
    "estimated_remediation_cost": 93750,
    "projected_annual_savings": 187500,
    "roi_percentage": 200
  },
  "debt_breakdown": {
    "code_quality": {
      "hours": 450,
      "percentage": 36,
      "top_issues": ["high_complexity", "code_duplication", "low_coverage"]
    },
    "architecture": {
      "hours": 380,
      "percentage": 30.4,
      "top_issues": ["tight_coupling", "layer_violations", "circular_dependencies"]
    },
    "security": {
      "hours": 220,
      "percentage": 17.6,
      "top_issues": ["outdated_dependencies", "weak_authentication"]
    },
    "performance": {
      "hours": 200,
      "percentage": 16,
      "top_issues": ["slow_queries", "large_bundles", "memory_leaks"]
    }
  },
  "priority_recommendations": [
    {
      "title": "Refactor payment processing module",
      "category": "architecture",
      "effort_hours": 40,
      "impact_score": 9.2,
      "roi_percentage": 450,
      "timeline": "immediate",
      "justification": "Critical path for new payment features, blocking 3 upcoming stories"
    },
    {
      "title": "Update authentication dependencies",
      "category": "security", 
      "effort_hours": 16,
      "impact_score": 8.8,
      "roi_percentage": 300,
      "timeline": "immediate",
      "justification": "Security vulnerability with known exploits"
    }
  ],
  "remediation_roadmap": {
    "phase_1_q1": {
      "focus": "Critical security and architecture debt",
      "effort_hours": 320,
      "expected_velocity_improvement": 8,
      "items": 12
    },
    "phase_2_q2": {
      "focus": "Code quality and performance optimization",
      "effort_hours": 400,
      "expected_velocity_improvement": 12,
      "items": 18
    }
  }
}
```

### ROI Analysis Dashboard
```json
{
  "investment_analysis": {
    "total_investment_hours": 800,
    "total_investment_cost": 60000,
    "annual_benefits": {
      "velocity_improvement": 90000,
      "bug_reduction_savings": 45000,
      "maintenance_cost_reduction": 30000,
      "total": 165000
    },
    "payback_period_months": 4.36,
    "3_year_npv": 425000,
    "break_even_confidence": 0.85
  },
  "velocity_modeling": {
    "current_velocity": 32,
    "projected_velocity_post_remediation": 39,
    "improvement_percentage": 21.875,
    "features_per_quarter_gain": 14
  }
}
```

## Success Metrics

### Technical Health Indicators
- **Debt Ratio Reduction**: Target <20% of codebase hours
- **Velocity Improvement**: +15% sprint velocity after remediation
- **Bug Rate Reduction**: -40% production incidents
- **Code Quality Scores**: All modules >7/10 maintainability

### Business Impact Metrics
- **Feature Delivery Speed**: +25% faster time-to-market
- **Development Cost Efficiency**: -20% cost per feature
- **Team Satisfaction**: +30% developer happiness scores
- **Technical Interview Success**: +50% candidate acceptance rate

## Integration Requirements

### Code Analysis Tools
- **SonarQube**: Code quality and security analysis
- **CodeClimate**: Maintainability and test coverage metrics
- **ESLint/TSLint**: Static code analysis for JavaScript/TypeScript
- **Dependabot**: Dependency vulnerability scanning

### Development Metrics
- **GitHub/GitLab**: PR velocity, review time, contributor activity
- **Jira/Linear**: Feature delivery speed, bug tracking
- **CI/CD Platforms**: Build success rates, deployment frequency

### Business Intelligence
- **Analytics Platforms**: User experience impact measurement
- **Cost Tracking**: Infrastructure and development cost correlation
- **Performance Monitoring**: Application performance trend analysis

The Technical Debt Auditor skill transforms subjective technical debt discussions into objective, ROI-driven decision making, enabling engineering managers to build compelling business cases for technical improvements while maximizing team productivity and code quality.