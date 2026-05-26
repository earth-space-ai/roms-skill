# ROMS Skill

Progressive-disclosure skill for the [Regional Ocean Modeling System](https://github.com/myroms/roms) (ROMS), a terrain-following, finite-difference, free-surface regional ocean model.

> **Skill author:** Koutian Wu (ktwu01@gmail.com)
> **Skill version:** 0.1.0-scaffold

> ⚠️ **Disclaimer — please read before using this skill.**
> This skill is **not a gold-standard reference**. It is a helper that lowers
> the barrier for new users to **get their hands dirty** with the model. AI
> agents (and the humans drafting this material) make mistakes; commands, file
> paths, namelist options, and physics explanations here can be wrong,
> incomplete, or out of date. **Always cross-check with the official model
> documentation, the source code, and a human expert before trusting any
> output for research, publication, or operational use.**

## What This Is

A guide to building, configuring, and running ROMS regional ocean simulations. Covers the application directory convention, the `ocean.in` namelist, compile-time CPP switches, and the coupling story (COAWST with WRF + SWAN, ESMF/NUOPC).

## Status

Scaffold. Layout verified against the cloned `myroms/roms` tree. Operational depth being filled in.

## License

MIT (skill content). ROMS has its own license; see `License_ROMS.md` upstream.
