# Contributing to OpenDirect MCP Implementation

Thank you for your interest in contributing to this project! This MCP implementation of the IAB Tech Lab OpenDirect v2.1 specification welcomes contributions from the community.

## How to Contribute

### Reporting Issues

If you find bugs, have questions, or want to suggest improvements:

1. Check existing [issues](https://github.com/YOUR_USERNAME/opendirect-mcp/issues) to avoid duplicates
2. Create a new issue with:
   - Clear, descriptive title
   - Detailed description
   - Steps to reproduce (for bugs)
   - Expected vs. actual behavior
   - Environment details (OS, versions, etc.)

### Submitting Changes

1. **Fork the repository**
   ```bash
   git clone https://github.com/YOUR_USERNAME/opendirect-mcp.git
   cd opendirect-mcp
   ```

2. **Create a feature branch**
   ```bash
   git checkout -b feature/your-feature-name
   ```

3. **Make your changes**
   - Follow existing code style
   - Update documentation as needed
   - Add examples if applicable
   - Ensure schema validity

4. **Test your changes**
   - Validate JSON schema
   - Check all examples work
   - Verify documentation accuracy

5. **Commit your changes**
   ```bash
   git add .
   git commit -m "Add: brief description of changes"
   ```

6. **Push to your fork**
   ```bash
   git push origin feature/your-feature-name
   ```

7. **Create a Pull Request**
   - Go to the original repository
   - Click "New Pull Request"
   - Select your branch
   - Describe your changes clearly

## Contribution Guidelines

### Code Style

- **JSON**: Use 2-space indentation
- **Markdown**: Follow standard markdown formatting
- **Comments**: Add explanatory comments for complex schemas
- **Naming**: Use clear, descriptive names matching OpenDirect spec

### Documentation

- Update README.md for major changes
- Add examples for new features
- Update diagrams if object relationships change
- Keep documentation in sync with schema

### Schema Changes

When modifying `opendirect-mcp-server.json`:

1. **Maintain backward compatibility** when possible
2. **Follow OpenDirect v2.1 spec** exactly
3. **Update version numbers** appropriately
4. **Document breaking changes** clearly
5. **Add migration guides** for breaking changes

### Commit Messages

Use clear, descriptive commit messages:

```
Add: New feature description
Fix: Bug description
Update: What was updated
Docs: Documentation changes
Refactor: Code restructuring
Test: Test additions or changes
```

## What We're Looking For

### High Priority

- Bug fixes in schema definitions
- Additional implementation examples
- More visual diagrams
- Translation to other languages
- Integration guides for specific platforms

### Medium Priority

- Performance optimizations
- Additional validation rules
- Extended documentation
- Use case examples
- Best practice guides

### Enhancement Ideas

- TypeScript type definitions
- Python type annotations
- Code generators
- Testing frameworks
- CLI tools
- Web-based schema validators

## Standards Compliance

All contributions must:

- ✅ Comply with OpenDirect v2.1 specification
- ✅ Follow IAB Tech Lab standards
- ✅ Maintain AdCOM compatibility
- ✅ Preserve OpenRTB integration
- ✅ Support ISO standards (639-1, 3166, 4217)

## Code of Conduct

### Our Pledge

We are committed to providing a welcoming and inclusive environment for all contributors.

### Expected Behavior

- Be respectful and considerate
- Welcome newcomers
- Accept constructive criticism
- Focus on what's best for the project
- Show empathy toward others

### Unacceptable Behavior

- Harassment or discriminatory language
- Personal attacks
- Trolling or insulting comments
- Publishing others' private information
- Other unprofessional conduct

## Getting Help

- **Questions**: Open an issue with the "question" label
- **Discussions**: Use GitHub Discussions for general topics
- **IAB Tech Lab**: Contact openmedia@iabtechlab.com for spec questions
- **Documentation**: Check the INDEX.md for guidance

## Review Process

1. **Automated checks**: Pull requests trigger validation
2. **Maintainer review**: At least one maintainer must approve
3. **Community feedback**: Other contributors may comment
4. **Merge**: Approved PRs are merged by maintainers

## Recognition

Contributors will be acknowledged in:
- CONTRIBUTORS.md file
- Release notes for significant contributions
- Project documentation

## License

By contributing, you agree that your contributions will be licensed under the same Creative Commons Attribution 3.0 License as the project.

## Additional Resources

- [OpenDirect Specification](https://iabtechlab.com/opendirect)
- [IAB Tech Lab](https://iabtechlab.com)
- [AdCOM Specification](https://github.com/InteractiveAdvertisingBureau/AdCOM)
- [OpenRTB Specification](https://github.com/InteractiveAdvertisingBureau/openrtb)

## Questions?

Feel free to open an issue or reach out to the maintainers. We're here to help!

---

Thank you for contributing to the OpenDirect MCP Implementation! 🎉
