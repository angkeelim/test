```mermaid
sequenceDiagram
    autonumber
    actor Developer as 👨‍💻 Developer (HSG)
    actor Approver as 👨‍🏫 TechOps (RYT Bank)
    participant Main as 🧩 Main branch
    participant feature_xx as 🌿 feature_xx branch

    %% ==== DEVELOPMENT PHASE ====
    Note over Developer, Main: <font color="DodgerBlue">**Phase 1: Development (Dev Env)**</font>

    Main ->> feature_xx: Create a feature branch (for development)
    Developer ->> feature_xx: Push locally modified code<br/>(self-tested & passed)
    feature_xx ->> feature_xx: GitHub Actions build & push image to ACR
    feature_xx ->> feature_xx: Update <b>dev-kubernetes-applications</b> deployment YAML<br/>Trigger ArgoCD deploy → <b>Dev Env</b> & Run Unit Tests

    %% ==== TEST / UAT PHASE ====
    Note over feature_xx: <font color="Orange">**Phase 2: Testing (UAT Env)**</font>

    feature_xx ->> feature_xx: Update <b>test-kubernetes-applications</b> deployment YAML<br/>Trigger ArgoCD deploy → <b>Test Env</b> & Run UAT Tests

    %% ==== APPROVAL & MERGE ====
    Note over Approver, Main: <font color="MediumSeaGreen">**Phase 3: Merge & Approval**</font>

    feature_xx ->> Main: Create Merge Request
    Approver ->> Main: Review & Approve Merge
    feature_xx ->> Main: Merge & delete feature branch

    %% ==== DEPLOYMENT TO PROD/DR ====
    Note over Main: <font color="Crimson">**Phase 4: Production / DR Deployment**</font>

    Main ->> Main: GitHub Actions build & push image to ACR
    Main ->> Main: Update <b>production-kubernetes-applications</b><br/>and/or <b>dr-kubernetes-applications</b> deployment YAML<br/>Trigger ArgoCD deploy → <b>Production / DR Env</b>
    Main ->> Main: Tag release (vX.X.X)
