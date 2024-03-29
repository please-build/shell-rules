subinclude("//build_defs:shell")

# This tests that data files exist in the correct location, and
# indirectly performs a basic test of sh_test which we don't use elsewhere.
sh_test(
    name = "data_files_test",
    src = "data_files_test.sh",
    data = ["container_data.txt"],
)

genrule(
    name = "pass_env",
    outs = ["pass_env.sh"],
    cmd = "echo \"export USER='$USER'\" > \"$OUT\"",
    pass_env = ["USER"],
)

sh_test(
    name = "pass_env_test",
    src = "pass_env_test.sh",
    data = [":pass_env"],
    labels = ["manual"],  # Unclear what a good env var to pass is that works locally & on CI.
)

sh_library(
    name = "sh_lib",
    src = "sh_lib.sh",
)

sh_binary(
    name = "sh_binary",
    main = "sh_binary.sh",
    deps = [
        ":sh_lib",
    ],
)

sh_test(
    name = "sh_test",
    src = ":sh_binary",
)

sh_cmd(
    name = "sh_cmd",
    cmd = [
        'BUILD_TIME_EXPANSION_VAR="$OUT"',
        'NO_EXPANSION_VAR="\\\\$@"',
        '[ "\\\\$BUILD_TIME_EXPANSION_VAR" != "" ] || (echo "Expected a build time var to be expanded." >&2 && exit 11)',
        '[ "\\\\$NO_EXPANSION_VAR" = "test_value" ] || (echo "Expected a expansion not to happen" >&2 && exit 11)',
    ],
)

sh_cmd(
    name = "sh_cmd_unexpanded",
    cmd = [
        'VAR="$OUT"',
        '[ "$VAR" = "" ] || (echo "Expected a build time var to be unexpanded." >&2 && exit 11)',
    ],
    expand_env_vars = False,
)

sh_test(
    name = "sh_cmd_test",
    src = ":sh_cmd",
    flags = "test_value",
)

sh_test(
    name = "sh_cmd_unexpanded_test",
    src = ":sh_cmd_unexpanded",
)

sh_binary(
    name = "no_shebang",
    main = "no_shebang.sh",
    deps = [
        # Although not used by `no_shebang.sh` directly, the shebang is currently reused when `deps` are provided.
        ":sh_lib",
    ],
)

sh_test(
    name = "no_shebang_test",
    src = ":no_shebang",
)

sh_binary(
    name = "shebang",
    main = "shebang.sh",
    deps = [
        # Although not used by `shebang.sh` directly, the shebang is currently reused when `deps` are provided.
        ":sh_lib",
    ],
)

sh_test(
    name = "shebang_test",
    src = ":shebang",
)
