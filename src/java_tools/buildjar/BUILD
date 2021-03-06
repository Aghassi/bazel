load("@rules_java//java:defs.bzl", "java_binary", "java_toolchain")

# Description:
# JavaBuilder and java tools used by Bazel
package(default_visibility = [":buildjar_package_group"])

filegroup(
    name = "srcs",
    srcs = [
        "BUILD",
        "//src/java_tools/buildjar/java/com/google/devtools/build/buildjar:srcs",
        "//src/java_tools/buildjar/java/com/google/devtools/build/java/bazel:srcs",
        "//src/java_tools/buildjar/java/com/google/devtools/build/java/turbine:srcs",
        "//src/java_tools/buildjar/javatests/com/google/devtools/build/buildjar:srcs",
        "//src/java_tools/buildjar/javatests/com/google/devtools/build/java/turbine:srcs",
    ],
    visibility = ["//src:__subpackages__"],
)

package_group(
    name = "buildjar_package_group",
    packages = [
        "//src/...",
        "//src/java_tools/buildjar/...",
        "//src/tools/android/...",
    ],
)

java_binary(
    name = "JavaBuilder",
    main_class = "com.google.devtools.build.buildjar.BazelJavaBuilder",
    visibility = ["//visibility:public"],
    runtime_deps = ["//src/java_tools/buildjar/java/com/google/devtools/build/buildjar"],
)

java_binary(
    name = "VanillaJavaBuilder",
    main_class = "com.google.devtools.build.buildjar.VanillaJavaBuilder",
    visibility = ["//visibility:public"],
    runtime_deps = ["//src/java_tools/buildjar/java/com/google/devtools/build/buildjar:vanilla_java_builder"],
)

filegroup(
    name = "bootstrap_VanillaJavaBuilder_deploy.jar",
    srcs = ["//src/java_tools/buildjar/java/com/google/devtools/build/buildjar:bootstrap_VanillaJavaBuilder_deploy.jar"],
    tags = ["manual"],
    visibility = ["//visibility:public"],
)

filegroup(
    name = "bootstrap_genclass_deploy.jar",
    srcs = ["//src/java_tools/buildjar/java/com/google/devtools/build/buildjar/genclass:bootstrap_genclass_deploy.jar"],
    tags = ["manual"],
    visibility = ["//visibility:public"],
)

# This toolchain is used to bootstrap Bazel.
java_toolchain(
    name = "bootstrap_toolchain",
    bootclasspath = ["@bazel_tools//tools/jdk:platformclasspath.jar"],
    genclass = ["bootstrap_genclass_deploy.jar"],
    ijar = ["//third_party/ijar"],
    javabuilder = ["bootstrap_VanillaJavaBuilder_deploy.jar"],
    jvm_opts = [
        # Prevent "Could not reserve enough space for object heap" errors on Windows.
        "-Xmx512m",
        # Using tiered compilation improves performance of Javac when not using the worker mode.
        "-XX:+TieredCompilation",
        "-XX:TieredStopAtLevel=1",
    ],
    singlejar = ["//src/java_tools/singlejar:bootstrap_deploy.jar"],
    source_version = "8",
    tags = ["manual"],
    target_version = "8",
    visibility = ["//visibility:public"],
)
