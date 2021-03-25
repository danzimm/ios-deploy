genrule(
    name = "gen-lldb.py.h",
    srcs = [
        "src/scripts/lldb.py",
    ],
    outs = [
        "lldb.py.h",
    ],
    cmd = 'awk \'{ print "\\""$$0"\\\\n\\""}\' $(SRCS) > \"$@\"',
)

objc_library(
    name = "ios-deploy-lib",
    srcs = glob(["src/ios-deploy/*.h", "src/ios-deploy/*.m"]) + [
        ":lldb.py.h",
    ],
    sdk_frameworks = [
        "CoreFoundation",
        "Foundation",
        "MobileDevice",
    ],
    copts = [
        "-fno-objc-arc",
        "-Oz",
    ],
)

apple_binary(
    name = "ios-deploy-lipobin",
    deps = [
        ":ios-deploy-lib",
    ],
    platform_type = "macos",
    linkopts = [
        # Older versions of macOS SDK have it here
        "-F/System/Library/PrivateFrameworks",
        # Newer versions of macOS SDK have it here
        "-F/Library/Apple/System/Library/PrivateFrameworks",
    ],
    visibility = ["//visibility:public"],
)

genrule(
    name = "ios-deploy-bin",
    srcs = [":ios-deploy-lipobin_lipobin"],
    outs = ["ios-deploy"],
    cmd = "cp $< $@",
    visibility = ["//visibility:public"],
)
