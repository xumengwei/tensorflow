package(default_visibility = ["//visibility:public"])

licenses(["notice"])  # Apache 2.0

load("//tensorflow:tensorflow.bzl",
     "tf_copts",
     "if_android",
     "if_not_mobile")

# LINKER_SCRIPT = ":android_version_script.lds"
LINKER_SCRIPT = "//tensorflow/contrib/android:jni/version_script.lds"

# bazel build //tensorflow/examples/rs_test:rs_benchmark --crosstool_top=//external:android/crosstool --host_crosstool_top=@bazel_tools//tools/cpp:toolchain --cpu=armeabi-v7a --copt=-D__ANDROID_TYPES_FULL__
cc_binary(
    name = "rs_benchmark",
    srcs = [
        "rs_benchmark.cc",
        "utils.h",
        "abort_wrapper.cc",
        # "//tensorflow/core:android_op_registrations_and_gradients",
    ],
    copts = tf_copts() + [
        "-fexceptions",
        "-ffunction-sections",
        "-fdata-sections",
    ],
    linkopts = if_android([
        "-pie",
        "-landroid",
        "-llog",
        "-ldl",
        "-lz",
        "-lm",
        "-z defs",
        "-Wl,--wrap=abort",
        "-Wl,--gc-sections",
        "-Wl,--version-script",
        LINKER_SCRIPT,
    ]),
    linkstatic = 1,
    deps = [
        "@gemmlowp//:gemmlowp",
        # "//tensorflow/core:android_tensorflow_lib_selective_registration",
        "//tensorflow/core/kernels:android_tensorflow_kernels_rs_training",
        # "//tensorflow/core:android_tensorflow_lib",
        LINKER_SCRIPT,
    ],
)