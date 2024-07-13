load(":refresh_compile_commands.bzl", "refresh_compile_commands")

# See README.md for interface.

# But if you aren't doing any cross-compiling for other platforms, the following can be a good default:
# bazel run @hedron_compile_commands//:refresh_all
refresh_compile_commands(
    name = "refresh_all",
)


# Stardoc users only: Depend on "@hedron_compile_commands//:bzl_srcs_for_stardoc" as needed.
# Why? Stardoc requires all loaded files to be listed as deps; without this we'd prevent users from running Stardoc on their code when they load from this tool in, e.g., their own workspace.bzl or wrapping macros.
filegroup(
    name = "bzl_srcs_for_stardoc",
    visibility = ["//visibility:public"],
    srcs = glob(["**/*.bzl"]) + [
        "@bazel_tools//tools:bzl_srcs",
    ],
)



########################################
# Implementation:
# If you are looking into the implementation, start with the overview in ImplementationReadme.md.

exports_files(["refresh.template.py", "check_python_version.template.py"])  # For implicit use by the refresh_compile_commands macro, not direct use.

# fix for c++ 20 modules
load("@com_github_rnburn_bazel_cpp20_modules//cc_module:defs.bzl", "cc_module", "cc_module_binary")

cc_module(
    name = "_Builtin_stddef_max_align_t",
    is_system = True,
)

cc_module(
    name = "std_config",
    is_system = True,
    deps = [
        ":_Builtin_stddef_max_align_t",
    ],
)

cc_module(
    name = "std",
    is_system = True,
    deps = [
        ":std_config",
        ":_Builtin_stddef_max_align_t",
    ],
)

cc_module_binary(
    name = "print_args",
    srcs = ["print_args.cpp"],
    deps = [":std"],
    visibility = ["//visibility:public"],
)
