# MACE dielectric response Model for bulk water

A MACE model developed in [Ref. 1](#1) predicts the dipole moment, polarization, and their real space derivatives of bulk water. The dataset is taken from [Ref. 2](#2).

The following repository and commit were used to generate the model:
- Repository: [git@github.com:venkatkapil24/mace.git](git@github.com:venkatkapil24/mace.git)
- Commit: `ea9178fc7d6cdb54eb850623e09000c5ae27243e`

The following CLI command was used:

```sh
mace_run_train \
    --name="MACE_dipole_pol" \
    --train_file="_INSERT_" \
    --test_file="_INSERT_" \
    --model="AtomicDipolesMACE" \
    --E0s="average" \
    --num_channels=32 \
    --max_L=2 \
    --r_max=5.0 \
    --loss="dipole" \
    --dipole_weight=1.0 \
    --polarizability_weight=1.0 \
    --dipole_key="REF_dipole"  \
    --polarizability_key="REF_polarizability" \
    --weight_decay=5e-10 \
    --clip_grad=1.0 \
    --batch_size=64 \
    --valid_batch_size=64 \
    --max_num_epochs=100 \
    --scheduler_patience=20 \
    --patience=50 \
    --eval_interval=1 \
    --ema \
    --error_table='DipoleRMSE' \
    --default_dtype="float64"\
    --device=cuda \
    --seed=123 \
    --restart_latest \
    --save_cpu \
    --compute_polarizability
```
## Evaluating the model 

Use the following CLI command to evaluate the dipole moment and the polarization:

```sh
mace_eval_mu_alpha \
  --configs="_INSERT_ \
  --model="MACE_dipole_pol.model" \
  --output="_INSERT_" \
  --device=cuda \
  --batch_size=10 \
```

Use the additional flag if you also want to estimate the spatial derivatives of the dipole moment and the polarization:

```sh
mace_eval_mu_alpha \
  --configs="_INSERT_ \
  --model="MACE_dipole_pol.model" \
  --output="_INSERT_" \
  --device=cuda \
  --batch_size=10 \
  --compute_dielectric_derivatives \
```

## References

1. Kapil, V., Kovács, D. P., Csányi, G., & Michaelides, A. (2023). First-principles spectroscopy of aqueous interfaces using machine-learned electronic and quantum nuclear effects. Faraday Discussions, 10.1039.D3FD00113J. [https://doi.org/10.1039/D3FD00113J](https://doi.org/10.1039/D3FD00113J)
2. Grisafi, A., Wilkins, D. M., Csányi, G., & Ceriotti, M. (2018). Symmetry-adapted machine learning for tensorial properties of atomistic systems. Physical Review Letters, 120(3), 036002. [https://doi.org/10.1103/PhysRevLett.120.036002](https://doi.org/10.1103/PhysRevLett.120.036002)
