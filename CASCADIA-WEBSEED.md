# Cascadia webseed support

Set ABUILD_WEBSEED_FETCHER=/usr/local/bin/apk-webseed in abuild.conf to
let abuild-fetch try a private LAN BitTorrent swarm for HTTP(S) sources.
Use the helper from [Spaceghost/apk-tools](https://github.com/Spaceghost/apk-tools/blob/master/doc/CASCADIA-WEBSEED.md)
and configure APK_WEBSEED_CATALOG and APK_WEBSEED_CACHE in the builder
environment.

The hook uses the APKBUILD checksum for the source, including the filename
specified by name::URL and DISTFILES_MIRROR requests. While holding the
normal distfile lock, abuild-fetch calls the helper as:

    apk-webseed fetch --checksum sha512:HEX URL TEMPORARY_PATH

For legacy checksums the prefix is sha256. The helper must verify the
checksum before returning success. On failure abuild-fetch resumes its
usual curl/wget path, using the existing HTTP partial download if present.
Normal APKBUILD verification still applies to all fetched files. A recipe
that does not yet declare a checksum uses the usual download path.
