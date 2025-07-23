# Mitigation Strategy Metadata Schema

## Core Technology Identity
| Column | Type | Description | Example |
|--------|------|-------------|---------|
| `technology_id` | STRING | Unique identifier (UUID) | `tech_2024_001_ammonia_fuel` |
| `technology_name` | STRING | Human-readable name | `Ammonia Fuel Cells for Maritime Transport` |
| `technology_type` | STRING | Technical category | `alternative_fuel`, `material_substitution`, `process_optimization` |
| `technology_category` | STRING | Broader classification | `energy_storage`, `industrial_process`, `carbon_removal` |
| `discovery_date` | DATE | First identification date | `2024-01-15` |
| `last_updated` | TIMESTAMP | Most recent data update | `2024-01-23T14:30:00Z` |

## Research Source Tracking
| Column | Type | Description | Example |
|--------|------|-------------|---------|
| `primary_papers` | JSON_ARRAY | Paper DOIs/identifiers | `["10.1038/s41586-2024-123", "arXiv:2401.1234"]` |
| `patent_references` | JSON_ARRAY | Patent numbers | `["US20240001234", "EP4567890A1"]` |
| `institutional_reports` | JSON_ARRAY | Report identifiers | `["IEA-2024-AMMONIA", "IRENA-MARITIME-001"]` |
| `source_diversity` | INTEGER | Number of independent research groups | `3` |
| `geographic_research_spread` | JSON_ARRAY | Countries with active research | `["USA", "Norway", "Japan", "Singapore"]` |

## Validation Metrics (0-1 Scale)
| Column | Type | Description | Calculation Method |
|--------|------|-------------|-------------------|
| `novelty_score` | FLOAT | Technology novelty vs existing solutions | Semantic similarity vs patent/paper corpus |
| `publication_momentum_score` | FLOAT | Research publication velocity | Citations/month in first 6 months |
| `patent_activity_score` | FLOAT | Patent filing activity | Patent filings per research group |
| `carbon_impact_score` | FLOAT | Potential carbon reduction impact | GtCO2eq/year potential vs sector total emissions |
| `technical_feasibility_score` | FLOAT | Technical readiness assessment | TRL level + engineering complexity |
| `economic_viability_score` | FLOAT | Cost competitiveness potential | LCOE vs incumbent technologies |
| `overall_priority_score` | FLOAT | Weighted composite score | `0.25*novelty + 0.20*momentum + 0.15*patents + 0.25*carbon + 0.15*feasibility` |

## Sector Classifications
| Column | Type | Description | Example |
|--------|------|-------------|---------|
| `gri_primary_sectors` | JSON_ARRAY | Primary GRI sector alignments | `["Oil & Gas", "Mining"]` |
| `gri_secondary_sectors` | JSON_ARRAY | Secondary GRI sector potential | `["Agriculture", "Financial Services"]` |
| `sbti_primary_pathways` | JSON_ARRAY | Primary SBTi pathway alignments | `["Maritime", "Power"]` |
| `sbti_secondary_pathways` | JSON_ARRAY | Secondary SBTi pathway potential | `["Steel", "Chemicals"]` |
| `cross_sector_applicability` | JSON_OBJECT | Cross-sector potential mapping | `{"high": ["Steel", "Chemicals"], "medium": ["Transport"], "emerging": ["Buildings"]}` |

## Impact Assessment
| Column | Type | Description | Example |
|--------|------|-------------|---------|
| `carbon_reduction_potential_min_gtco2eq` | FLOAT | Minimum annual CO2 reduction potential | `0.8` |
| `carbon_reduction_potential_max_gtco2eq` | FLOAT | Maximum annual CO2 reduction potential | `2.1` |
| `trl_level_current` | INTEGER | Current Technology Readiness Level | `5` |
| `trl_level_target` | INTEGER | Target TRL for commercialization | `8` |
| `deployment_timeline_years` | INTEGER | Estimated years to commercial deployment | `3` |
| `capex_vs_incumbent_ratio` | FLOAT | Capital cost vs incumbent (1.0 = equal) | `1.2` |
| `opex_vs_incumbent_ratio` | FLOAT | Operating cost vs incumbent (1.0 = equal) | `0.85` |
| `market_size_billion_usd` | FLOAT | Total addressable market size | `145.6` |

## Research Momentum Tracking
| Column | Type | Description | Example |
|--------|------|-------------|---------|
| `citation_velocity_6m` | FLOAT | Citations per month (first 6 months) | `2.3` |
| `publication_count_total` | INTEGER | Total related publications | `15` |
| `publication_count_2024` | INTEGER | Publications in current year | `8` |
| `patent_filing_count_total` | INTEGER | Total patent filings | `12` |
| `patent_filing_count_recent` | INTEGER | Patent filings (last 12 months) | `4` |
| `funding_total_million_usd` | FLOAT | Total committed funding | `850.0` |
| `funding_recent_million_usd` | FLOAT | Recent funding (last 12 months) | `250.0` |

## Regulatory & Geographic Context
| Column | Type | Description | Example |
|--------|------|-------------|---------|
| `regulatory_status` | JSON_OBJECT | Regulatory approval status by region | `{"EU": "taxonomy_eligible", "USA": "under_review", "Asia": "pilot_approved"}` |
| `deployment_regions` | JSON_ARRAY | Regions with active deployments | `["Europe", "North America", "Singapore"]` |
| `policy_support_level` | STRING | Government policy support | `high`, `medium`, `low`, `none` |
| `standards_compliance` | JSON_ARRAY | Relevant standards compliance | `["ISO14040", "GHG_Protocol", "SBTi_Maritime"]` |

## Temporal Evolution Tracking
| Column | Type | Description | Example |
|--------|------|-------------|---------|
| `validation_history` | JSON_ARRAY | Historical validation scores | `[{"date": "2024-01-15", "novelty": 0.85, "momentum": 0.72}, {...}]` |
| `trl_progression` | JSON_ARRAY | TRL evolution over time | `[{"date": "2023-06-01", "trl": 4}, {"date": "2024-01-15", "trl": 5}]` |
| `funding_timeline` | JSON_ARRAY | Chronological funding events | `[{"date": "2023-09-15", "amount": 50.0, "source": "DOE_Grant"}, {...}]` |
| `milestone_events` | JSON_ARRAY | Key development milestones | `[{"date": "2024-01-10", "event": "pilot_deployment", "location": "Port_of_Rotterdam"}]` |

## Quality Control & Metadata
| Column | Type | Description | Example |
|--------|------|-------------|---------|
| `data_quality_score` | FLOAT | Overall data completeness/reliability | `0.91` |
| `confidence_interval` | FLOAT | Statistical confidence in assessments | `0.85` |
| `validation_method` | STRING | How the technology was validated | `multi_source_triangulation`, `expert_review`, `automated_only` |
| `last_expert_review` | DATE | Date of last human expert validation | `2024-01-20` |
| `data_sources_count` | INTEGER | Number of data sources contributing | `8` |
| `contradictory_findings` | BOOLEAN | Whether sources show contradictory results | `false` |

## Archive Management
| Column | Type | Description | Example |
|--------|------|-------------|---------|
| `archive_version` | STRING | Version control for this entry | `v1.3.2` |
| `created_by` | STRING | System/process that created entry | `pocketflow_daily_v2.1` |
| `modified_by` | STRING | Last modification source | `expert_validation_system` |
| `public_visibility` | BOOLEAN | Whether entry is publicly visible | `true` |
| `data_license` | STRING | Data usage license | `CC-BY-4.0` |