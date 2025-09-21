Unconditional RH wrapper blockers (as of 2025-09-21)

Scope: Adding `theorem RH.Proof.Unconditional.RiemannHypothesis_unconditional : RiemannHypothesis` without new axioms or sorries.

Status: Blocked. The following ingredients are not available unconditionally in the current codebase and are required to assemble the zero-argument theorem via the pinch route.

Missing lemmas/statements (exact shapes and suggested homes):

1) Half-plane/subset Poisson representation for the pinch field
   - Expected: existence on the off-zeros set using the RS outer existence and boundary bound.
   - Statement:
     `theorem RH.AcademicFramework.HalfPlaneOuterV2.pinch_poissonRepOn_offZeros
        (hDet2 : RH.RS.Det2OnOmega)
        {O : ℂ → ℂ} (hO : RH.RS.OuterHalfPlane O)
        (hBME : RH.RS.BoundaryModulusEq O (fun s => RH.RS.det2 s / RH.AcademicFramework.CompletedXi.riemannXi_ext s))
        (hXi : AnalyticOn ℂ RH.AcademicFramework.CompletedXi.riemannXi_ext RH.RS.Ω)
        (hDet_meas : Measurable (fun t : ℝ => RH.RS.det2 (RH.RS.boundary t)))
        (hO_meas   : Measurable (fun t => O (RH.RS.boundary t)))
        (hXi_meas  : Measurable (fun t => RH.AcademicFramework.CompletedXi.riemannXi_ext (RH.RS.boundary t)))
        : RH.AcademicFramework.HalfPlaneOuterV2.HasPoissonRepOn
            (RH.RS.F_pinch RH.RS.det2 O)
            (RH.RS.Ω \ {z | RH.AcademicFramework.CompletedXi.riemannXi_ext z = 0})`
   - Note: A closely matching theorem exists at `rh/academic_framework/HalfPlaneOuterV2.lean` (pinch_poissonRepOn_offZeros), but we still need unconditional inputs `hDet2`, `hXi` and the measurability facts to be available in a zero-arg path.

2) Boundary positivity (P+) source for the concrete pinch field
   - Expected: (P+) for `F := 2 · J_pinch det2 O` from a concrete Carleson budget.
   - Statement:
     `theorem RH.RS.PPlusFromCarleson_exists_proved
        : RH.Cert.PPlusFromCarleson_exists (fun z => (2 : ℂ) * RH.RS.J_pinch RH.RS.det2 O z)`
   - Suggested file: `rh/RS/PPlusFromCarleson.lean` (there is a façade, but no proof term exporting the existential production). Alternatively expose an unconditional witness from `RH.Cert` sufficient to feed `RH.Cert.PPlusFromCarleson_exists` for the concrete `F`.

3) Unconditional pinned data at ξ_ext zeros or a builder to reach removable assignment
   - We have: `RH.RS.PinnedRemovable.removable_assign_for_Theta_pinch_ext` once a pinned u-trick dataset is supplied.
   - Missing: a zero-argument theorem producing the pinned dataset for `Θ_pinch_of det2 (choose O)` at each ξ_ext-zero. If this is intended to come from a separate verified track, add a statement-level provider.

4) Zero-arg outer existence connection to Poisson–Cayley transport
   - We have: `RH.RS.Det2Outer.outer_limit_locally_uniform : RH.RS.OuterHalfPlane.ofModulus_det2_over_xi_ext`.
   - To complete the chain we still need unconditional analyticity/measurability hooks used by (1).

Minimal next steps:
 - Implement an unconditional `PPlusFromCarleson_exists_proved` in `rh/RS/PPlusFromCarleson.lean`, sourcing the budget from `RH.Cert.kxiWitness_nonempty` and the adapters in `rh/RS/BoundaryWedge.lean` and `rh/RS/CRGreenOuter.lean`.
 - Expose a zero-argument `Det2OnOmega` witness or a sufficient analytic/measurability bundle for `det2` and `xi_ext` on `Ω` to satisfy `pinch_poissonRepOn_offZeros` inputs.
 - Provide a zero-argument pinned u-trick dataset for `Θ_pinch_of det2 (choose O)` across ξ_ext zeros, or a thin statement-level adapter that delegates to existing pinned sources.

Until these are in place, `theorem RH.Proof.Unconditional.RiemannHypothesis_unconditional : RiemannHypothesis` cannot be added without introducing new assumptions.


