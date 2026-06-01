# CKA Reddit & Community Feedback (2025-2026)

## Common Pain Points from r/kubernetes & Community Feedback

This document tracks recurring feedback themes from CKA candidates on Reddit, forums, and community channels. Each improvement is tied to an exercise or study material.

---

## 🔴 Red Flag Exercises (Most Commonly Failed)

### Domain: RBAC & Authorization (Exercise 04)
**Community Feedback:**
> "I studied RBAC for days and still got it wrong. The exam was way more complex than any practice question." — reddit.com/r/kubernetes

**Issue:** Role vs ClusterRole confusion, verb/resource combinations, aggregated roles
**Improvement Target:** 
- Add sub-exercises for: `create` vs `get`, wildcard permissions, resource scoping
- Document common RBAC mistakes (e.g., `*` doesn't include subresources like `*/scale`)
- Add real exam scenario: troubleshoot "User can pod exec but not pod delete"

**Status:** ⏳ Ready to enhance `exercises/04-rbac/README.md`

---

### Domain: NetworkPolicy (Exercise 05, 28)
**Community Feedback:**
> "NetworkPolicy feels broken. My deny-all policy didn't work, then I realized my CNI doesn't enforce it." — reddit

**Issue:** 
- Not all CNIs enforce NetworkPolicy (Flannel doesn't, Calico does)
- Ingress vs Egress direction confusion
- Label selector syntax tricky with operators (matchLabels vs matchExpressions)
- Lack of debugging guidance (how to verify if policy is actually enforced)

**Improvement Target:**
- Add CNI compatibility matrix to Exercise 05
- Document `kubectl get netpol` doesn't show enforcement (need to check CNI logs)
- Add real failure: "Create deny-all, but traffic still flows" diagnostic
- Address feedback: "I need to know which CNI my cluster uses"

**Status:** ⏳ Ready to enhance `exercises/05-networkpolicy/README.md` & `exercises/28-network-policy-complex/README.md`

---

### Domain: Storage & PV/PVC (Exercise 12)
**Community Feedback:**
> "PVC binding is not intuitive. The exercise didn't explain why my PVC is stuck in Pending." — reddit
> "Storage classes are underrated; I got multiple storage questions on exam" — reddit

**Issue:**
- PVC Pending state investigation (access modes, storage class, node affinity)
- Storage class selectors not obvious
- ReclaimPolicy affects whether PV can be reused
- Multiple storage backends (local, nfs, ceph) confuse new users

**Improvement Target:**
- Add section: "Why is my PVC Pending?" troubleshooting flowchart
- Document ReclaimPolicy behavior clearly
- Add exercise: Create PVC, fail, fix by adding storage class
- Real exam pattern: "Scale app from 1 to 3 replicas, PVCs fail because access mode is RWO not RWX"

**Status:** ⏳ Ready to enhance `exercises/12-storage-pv-pvc/README.md`

---

### Domain: HPA & Metrics (Exercise 16)
**Community Feedback:**
> "HPA is in the exam but my metrics-server was broken. Took me 10 minutes to debug." — reddit
> "I got an HPA question but target.type=Utilization confused me" — reddit

**Issue:**
- Metrics server installation/verification not obvious
- CPU/memory metrics not available if requests not set on pod
- target.type: Utilization vs Value requires understanding pod resource requests
- `kubectl top` command returns "metrics not available" — debugging unclear

**Improvement Target:**
- Add pre-flight checklist: "Is metrics-server running? Are pod requests set?"
- Document exact CPU/memory formula for target utilization
- Add troubleshooting: "kubectl top shows no data" diagnostic steps
- Real pattern: "HPA created but not scaling — metrics are the culprit"

**Status:** ⏳ Ready to enhance `exercises/16-hpa/README.md`

---

## ⚠️ Time Management Feedback

### Most Time-Consuming Mistakes (From Community)
1. **Debugging RBAC** (avg 12 min lost) — should take 3-4 min
2. **NetworkPolicy testing** (avg 10 min lost) — unsure if policy is actually enforced
3. **Storage troubleshooting** (avg 8 min lost) — multiple potential causes
4. **Certificate/TLS issues** (avg 7 min lost) — hard to debug without logs

**Improvement:** Add "Quick Diagnostics" section to each exercise
- Example: "If X fails, check Y command first"
- Mapping: symptom → diagnosis command → fix

---

## 📊 Exercise Readiness Assessment

### Exercises Candidates Say Are Well-Covered
✅ Pod basics (01)
✅ Deployment rollout (06)
✅ Static pods (10)
✅ Jobs & CronJobs (21)

### Exercises Candidates Say Need More Content
❌ RBAC edge cases (04) — ask for more examples
❌ NetworkPolicy debugging (05, 28) — "I need to test this"
❌ Storage access modes (12) — "Why RWO vs RWX?"
❌ HPA metrics setup (16) — "Is metrics-server running?"
❌ kubeadm upgrade (09) — "Complex, needs more walkthrough"
❌ Ingress (19) — "Multiple backends, need clarification"

---

## 🚀 Recommended Improvements (Priority Order)

### Tier 1 (Highest Impact) — Add to Exercises

**1. RBAC (Exercise 04) Enhanced Gotchas**
- Subresources gotcha: `verbs: ["get", "list"]` doesn't grant `get/scale` access
- Wildcard gotcha: `resources: ["*"]` doesn't mean all subresources
- Common mistake: Forgetting `apiGroups: [""]` for core API resources

**2. Storage (Exercise 12) Diagnostic Flowchart**
- PVC Pending? flowchart: Check access mode → storage class → node affinity

**3. NetworkPolicy (Exercise 05, 28) CNI Awareness**
- Document CNI enforcement behavior
- Add: "How to verify policy is actually enforced"

**4. HPA (Exercise 16) Metrics Pre-Flight Check**
- Checklist before debugging HPA issues
- Exact command: `kubectl get deployment -o json | jq '.spec.template.spec.containers[].resources'`

**5. Troubleshooting (Exercise 11, 29) Real Exam Scenarios**
- Add section: "Questions from Reddit about what was on their exam"
- Example: "My node kept failing health checks, turned out to be DNS in kubelet config"

### Tier 2 (Medium Impact) — Documentation

**6. DOMAINS.md Enhancement**
- Add: "Domains Reddit users got wrong most" (RBAC, Networking)
- Adjust time recommendations based on difficulty feedback

**7. Mock Exams README**
- Add: "Which exercises most likely to appear on real exam?"
- Based on community reports

---

## 📝 Community-Suggested New Exercises

### From Reddit Requests:
1. **Exercise: LSO (Local Storage Operator)** — Many candidates asked for local storage guidance
2. **Exercise: DaemonSet advancement** — Quiet topic but appears on exam
3. **Exercise: Pod disruption budgets** — "Didn't see this coming on exam"
4. **Exercise: Scheduler configuration** — "How to implement custom affinity rules?"

**Status:** 📋 Consider adding to future versions based on usage

---

## Feedback Collection Framework

### How to Contribute:
1. If you see CKA feedback on Reddit/forums, add it here
2. Format: (Feedback text) → (Exercise #) → (Suggested fix)
3. Tag with 📅 date and 🔗 source link if possible

### Next Steps:
- Monitor r/kubernetes for weekly mentions of CKA issues
- Update DOMAINS.md with real exam difficulty distribution
- Create REDDIT_FEEDBACK.md (this file) as living document
- Implement Tier 1 improvements before next release

---

## 🎯 Success Metrics

After implementing improvements, measure:
- ✅ GRADING.md exercise scores improve (track before/after)
- ✅ Candidate feedback mentions "This matched the real exam" more often
- ✅ Fewer "I wasn't prepared for..." comments
- ✅ More "This exercise was spot-on" comments

---

**Last Updated:** May 31, 2026
**Maintained by:** Community feedback loop
**Next Review:** June 14, 2026
