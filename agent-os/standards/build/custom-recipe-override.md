# Custom Recipe Override Per Target

`generate_rule` in `skel.mk` auto-generates a build rule for every
target using its target group's default recipe:

```make
define generate_rule
ifndef ${1}_CUSTOM_RECIPE
${1}: ${2} ${3} ; ...
endif
endef
```

- Defining `${TARGET}_CUSTOM_RECIPE` for a target (in its `Rules.mk`
  block) makes `generate_rule` skip emitting the default rule entirely
  — `Rules.mk` must then supply the full recipe itself
- Use this only when a target's compile command doesn't fit the
  standard target-group recipe (e.g. non-standard CL/DDS handling) —
  not as a general escape hatch, since a custom recipe won't get the
  automatic dependency/escaping wiring `generate_rule` normally provides
