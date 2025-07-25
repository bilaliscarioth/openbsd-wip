# ring-v0.17 does not support this arch
NOT_FOR_ARCHS =	sparc64

COMMENT =	next-generation test runner for Rust

VERSION =	0.9.101

DISTNAME =	cargo-nextest-${VERSION}

CATEGORIES =	devel

# https://github.com/nextest-rs/nextest/
HOMEPAGE =	https://nexte.st/

MAINTAINER =	Laurent Cheylus <foxy@free.fr>

SITES =		https://github.com/nextest-rs/nextest/
DISTFILES =	cargo-nextest-{archive/refs/tags/cargo-nextest-}${VERSION}.tar.gz

# MIT
PERMIT_PACKAGE =	Yes

WANTLIB +=	${MODCARGO_WANTLIB} m zstd

BUILD_DEPENDS+=	devel/cargo-generate-vendor \
		archivers/zstd
LIB_DEPENDS +=	archivers/zstd

MODULES =	devel/cargo

CONFIGURE_STYLE =	cargo

MODCARGO_RUSTFLAGS +=	-L${LOCALBASE}/lib

# Disable feature for self-update
MODCARGO_NO_DEFAULT_FEATURES =	Yes
MODCARGO_FEATURES =		default-no-update

# Crate uuid v1.2.1 needed for tests
MODCARGO_CRATES +=	uuid	1.2.1	# Apache-2.0 OR MIT

SEPARATE_BUILD =	Yes

post-extract:
	mv ${WRKDIR}/nextest-cargo-nextest-${VERSION}/ ${WRKSRC}
	rm -rf ${WRKDIR}/nextest-cargo-nextest-${VERSION}

do-install:
	${INSTALL_PROGRAM} ${MODCARGO_TARGET_DIR}/release/cargo-nextest ${PREFIX}/bin/

# Nextest's own tests do not work with cargo test.
# We must use nextest to run its own test suite.
#
# Summary [ 365.562s] 340 tests run: 329 passed (1 slow), 11 failed, 1 skipped
# SLOW [ 106.194s] integration-tests::integration test_run_no_tests
# FAIL [  22.107s] integration-tests::integration test_list_binaries_only
# FAIL [  21.681s] integration-tests::integration test_list_default
# FAIL [  21.679s] integration-tests::integration test_list_full
# FAIL [  11.408s] integration-tests::integration test_list_with_default_filter
# FAIL [  10.046s] integration-tests::integration test_run
# FAIL [  39.643s] integration-tests::integration test_run_with_default_filter
# FAIL [  37.502s] integration-tests::integration test_run_with_priorities
# FAIL [  10.280s] integration-tests::integration test_show_config_test_groups
# FAIL [   3.261s] integration-tests::integration test_target_dir
# FAIL [   0.260s] integration-tests::integration test_version_info
# FAIL [  19.880s] nextest-runner::integration basic::test_retries::retry_overrides_obeyed
do-test:
	NEXTEST_HIDE_PROGRESS_BAR=1 ${MODCARGO_CARGO_RUN} run \
		--package cargo-nextest -- nextest run --profile ci

.include "crates.inc"

.include <bsd.port.mk>
