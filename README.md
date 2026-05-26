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

## Acknowledgments

**Gold-standard references for ROMS** (use these to cross-check anything in this skill):
- ROMS project portal: https://www.myroms.org/
- myroms/roms repository: https://github.com/myroms/roms
- ROMS wiki (documentation hub): https://www.myroms.org/wiki/
- COAWST coupled modeling system: https://github.com/jcwarner-usgs/COAWST

This scaffold exists only because of the work of other people, and any value
it has is borrowed from theirs.

- The **ROMS developer community** at Rutgers IMCS, UCLA, and partner
  institutions for building and maintaining
  [myroms/roms](https://github.com/myroms/roms), the application directory
  convention, the `ocean.in` namelist, the CPP-switch build, and the
  myroms.org documentation portal.
- The **COAWST** project (Warner et al.) for the coupled
  ROMS+WRF+SWAN workflow this skill points users toward.
- The **ESMF / NUOPC** community for the coupling layer this skill uses for
  multi-component runs.
- **Zesen Huang** for [laps-skill](https://github.com/huangzesen/laps-skill),
  the progressive-disclosure layout this repo borrows.

Any errors, oversimplifications, or out-of-date claims in this skill are the
skill author's responsibility, not the upstream community's. This is a
scaffold; operational depth is being filled in iteratively.

## License

MIT (skill content). ROMS has its own license; see `License_ROMS.md` upstream.
