AUTOMAKE_OPTIONS = foreign
SUBDIRS = src man lib/gitsh vendor
EXTRA_DIST = LICENSE README.md
TESTS = spec/units/internal_command_spec.rb \
				spec/integration/arguments_spec.rb \
				spec/integration/cd_command_spec.rb \
				spec/integration/chaining.rb \
				spec/integration/coloring_spec.rb \
				spec/integration/create_repository_spec.rb \
				spec/integration/default_command_spec.rb \
				spec/integration/error_handling_spec.rb \
				spec/integration/escaping_spec.rb \
				spec/integration/greeting_spec.rb \
				spec/integration/persistent_history_spec.rb \
				spec/integration/prompt_spec.rb \
				spec/integration/shell_commands_spec.rb \
				spec/integration/tab_completion_spec.rb \
				spec/integration/variables_spec.rb \
				spec/support/colors.rb \
				spec/support/delegate_matcher.rb \
				spec/support/fake_readline.rb \
				spec/support/file_system.rb \
				spec/support/gitsh_runner.rb \
				spec/support/parser.rb \
				spec/support/working_directory.rb \
				spec/units/cli_spec.rb \
				spec/units/completer_spec.rb \
				spec/units/environment_spec.rb \
				spec/units/git_command_spec.rb \
				spec/units/git_repository_spec.rb \
				spec/units/history_spec.rb \
				spec/units/internal_command_spec.rb \
				spec/units/interpreter_spec.rb \
				spec/units/parser_spec.rb \
				spec/units/prompter_spec.rb \
				spec/units/readline_blank_filter_spec.rb \
				spec/units/shell_command_spec.rb \
				spec/units/transformer_spec.rb \
				spec/units/tree_spec.rb
TEST_EXTENSIONS = .rb
RB_LOG_COMPILER = rspec

.PHONY: release \
	release_build release_push release_clean \
	release_build_tarball release_build_homebrew release_build_arch \
	release_push_tarball release_push_homebrew release_push_arch \
	release_clean_tarball release_clean_homebrew release_clean_arch

edit_package = sed \
	-e 's|@PACKAGE[@]|$(PACKAGE)|g' \
	-e 's|@PACKAGE_VERSION[@]|$(PACKAGE_VERSION)|g' \
	-e 's|@DIST_ARCHIVES[@]|$(DIST_ARCHIVES)|g' \
	-e 's|@DIST_SHA[@]|$(DIST_SHA)|g'

release: release_build release_push release_clean

release_build: release_build_tarball release_build_homebrew release_build_arch

release_push: release_push_tarball release_push_homebrew release_push_arch

release_clean: release_clean_tarball release_clean_homebrew release_clean_arch

release_build_tarball: Makefile distcheck
	git clone --branch gh-pages git@github.com:thoughtbot/gitsh.git gh-pages
	cp $(DIST_ARCHIVES) gh-pages
	cd gh-pages &&\
		git add $(DIST_ARCHIVES) &&\
		git commit -m "Release version $(PACKAGE_VERSION)"

release_push_tarball:
	cd gh-pages &&\
		git push

release_clean_tarball:
	rm -rf gh-pages

release_build_homebrew: DIST_SHA
	git clone git@github.com:thoughtbot/homebrew-formulae.git homebrew-formulae
	$(edit_package) homebrew/gitsh.rb.in > homebrew-formulae/Formula/gitsh.rb
	cd homebrew-formulae &&\
		git commit -am "gitsh: Release version $(PACKAGE_VERSION)"

release_push_homebrew:
	cd homebrew-formulae &&\
		git push

release_clean_homebrew:
	rm -rf homebrew-formulae

release_build_arch: DIST_SHA
	git clone --branch master git@github.com:thoughtbot/gitsh.git release-arch
	cd release-arch &&\
		$(edit_package) arch/PKGBUILD.in > arch/PKGBUILD &&\
		git commit -m "Arch package for version $(PACKAGE_VERSION)" -- arch/PKGBUILD &&\
		git tag v$(PACKAGE_VERSION)

release_push_arch:
	cd release-arch &&\
		git push --tags &&\
		git push

release_clean_arch:
	rm -rf release-arch

DIST_SHA: Makefile distcheck
	$(eval DIST_SHA := $(shell shasum $(srcdir)/$(DIST_ARCHIVES) | cut -d' ' -f1))
