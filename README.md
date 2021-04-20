## [Documentation](https://exploreasl.github.io/Documentation/)

This repository includes the interactive GitHub pages documentation of ExploreASL.

### Required libraries

- Documentation [mkdocs](https://www.mkdocs.org/)
- Documentation [mike](https://github.com/jimporter/mike)

### Build process

- Update `gh-pages` using `mike deploy --push --update-aliases 1.x.x latest`.
- Update `default` using `mike set-default --push latest`.
- **No** need to run `mkdocs build` or other commands.
- Testing can be done using `mike serve`.

